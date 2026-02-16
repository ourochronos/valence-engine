# PostgreSQL + Rust Sidecar Architecture

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [rust-engine], [triples-atomic], [topology-embeddings], [graph-vector-duality]

## Decision

**PostgreSQL is persistence. Rust is compute.**

Stop trying to make PostgreSQL do everything. Split responsibilities cleanly:

### PostgreSQL: Durable Triple Store
- Triple table with SPO/POS/OSP indexes (3 covering indexes for any query pattern)
- Source provenance (sessions, timestamps, metadata)
- Supersession chains (temporal validity)
- Write-optimized: insert triples, update access timestamps, record links
- Read pattern: bulk load into sidecar on startup, incremental sync after

PostgreSQL does what it's brilliant at: **durable, ACID, indexed storage**.

### Rust Sidecar: Compute Engine
- Holds graph in memory as adjacency lists for O(1) traversal
- Computes topology embeddings in-process (spectral/walks/message-passing)
- Runs ANN search (HNSW, built in Rust)
- Handles dynamic confidence computation from graph topology
- Processes fusion queries (vector + graph + confidence in one pass)
- Manages working set and context assembly

Rust does what it's brilliant at: **fast in-memory computation with predictable latency**.

## Why This Works

### PostgreSQL Strengths
- ✅ Proven durability (decades of production use)
- ✅ ACID transactions (for provenance and supersession chains)
- ✅ Indexing (B-tree indexes on triple components)
- ✅ Operational maturity (backup, replication, tooling)
- ✅ Ecosystem fit (most users already have Postgres)

### PostgreSQL Weaknesses
- ❌ Graph traversal (multi-hop requires recursive CTEs — slow)
- ❌ Vector search (pgvector is good but not as fast as purpose-built)
- ❌ In-memory graph (can't hold adjacency lists, always disk-bound)
- ❌ Topology computation (can't run spectral embeddings in SQL)

### Rust Sidecar Strengths
- ✅ Memory-resident graph (adjacency lists in HashMap, O(1) access)
- ✅ Purpose-built ANN (HNSW implementation tuned for our data)
- ✅ Fast topology computation (Eigen decomposition, random walks)
- ✅ Single-pass fusion (one query touches vector + graph + confidence)
- ✅ Embeddable (can run as library, sidecar, or standalone)

### Rust Sidecar Weaknesses
- ❌ Not durable (crash = rebuild from Postgres)
- ❌ No built-in replication (Postgres handles this)
- ❌ Requires startup time (load graph from Postgres)

## Architecture

```
┌──────────────────────────────────────────┐
│  MCP / HTTP / stdio                       │  Protocol
├──────────────────────────────────────────┤
│  Rust Sidecar (Compute Engine)           │
│  ┌────────────────────────────────────┐  │
│  │ Adjacency lists (in-memory graph)  │  │
│  │ HNSW index (vector search)         │  │
│  │ Topology embeddings (computed)     │  │
│  │ Fusion query engine                │  │
│  │ Dynamic confidence (from topology)  │  │
│  └────────────────────────────────────┘  │
│              ↕ sync protocol              │
├──────────────────────────────────────────┤
│  PostgreSQL (Persistence)                │
│  ┌────────────────────────────────────┐  │
│  │ triples (SPO/POS/OSP indexes)      │  │
│  │ sources (provenance, timestamps)   │  │
│  │ supersessions (temporal chains)    │  │
│  └────────────────────────────────────┘  │
└──────────────────────────────────────────┘
```

## Data Flow

### Write Path
1. LLM decomposes NL → triples
2. Rust sidecar inserts into Postgres (transactional)
3. Postgres durably stores + indexes
4. Rust updates in-memory graph structures
5. Topology embeddings marked for recomputation (lazy)

### Read Path
1. Query arrives at Rust sidecar
2. Fusion query: vector search (HNSW) → candidate nodes
3. Graph traversal (adjacency lists) → precise neighborhood
4. Confidence computation (topology analysis) → ranking
5. Return assembled context

### Startup
1. Rust sidecar starts, connects to Postgres
2. Bulk load: `SELECT * FROM triples` → build adjacency lists
3. Compute initial topology embeddings (or load cached)
4. Build HNSW index from embeddings
5. Ready to serve queries

### Sync Protocol
- Postgres is source of truth
- Periodic sync: Rust pulls changes since last timestamp
- Incremental update: new triples → update adjacency lists → mark embeddings dirty
- Crash recovery: restart sidecar, rebuild from Postgres

## Implementation: Build on petgraph

[petgraph](https://docs.rs/petgraph/) is the standard Rust graph library. We layer triple semantics on top:

```rust
use petgraph::graph::{Graph, NodeIndex};
use std::collections::HashMap;

struct TripleGraph {
    graph: Graph<String, String>,  // nodes=entities, edges=relationships
    entity_index: HashMap<String, NodeIndex>,  // entity→node lookup
    spo_index: HashMap<(String, String, String), EdgeIndex>,  // triple lookup
}

impl TripleGraph {
    fn insert_triple(&mut self, s: &str, p: &str, o: &str) {
        let s_node = self.get_or_create_node(s);
        let o_node = self.get_or_create_node(o);
        let edge = self.graph.add_edge(s_node, o_node, p.to_string());
        self.spo_index.insert((s.into(), p.into(), o.into()), edge);
    }
    
    fn neighbors(&self, entity: &str) -> Vec<(String, String)> {
        // O(1) lookup via adjacency list
        if let Some(&node) = self.entity_index.get(entity) {
            self.graph.neighbors(node)
                .map(|n| (self.graph[n].clone(), /* relationship */))
                .collect()
        } else {
            vec![]
        }
    }
}
```

petgraph gives us:
- Efficient adjacency list storage
- Graph traversal algorithms (BFS, DFS, shortest paths)
- Iterators over neighbors, edges, subgraphs
- Mature, well-tested, zero-copy where possible

We add:
- Triple indexing (SPO/POS/OSP in-memory indexes)
- Topology embeddings (spectral, random walks)
- HNSW vector index (separate structure, points into graph)
- Sync protocol with Postgres

## Why Not All-Postgres or All-Rust?

### Why Not All-Postgres?
- pgvector + Apache AGE = extensions that don't compose well
- Graph traversal in SQL is painful (recursive CTEs, slow)
- Can't compute spectral embeddings in Postgres
- Fusion queries would be multi-pass (vector query, then graph query, then join)

### Why Not All-Rust?
- Reinventing database durability is hard (write-ahead logs, crash recovery, backups)
- Operational burden (users expect Postgres, have tooling for it)
- No ACID (transactional triples + provenance + supersessions requires transactions)
- Replication story is complex (would need to build Raft or similar)

### This Split: Best of Both
- Postgres handles what databases do best (durable, indexed, transactional storage)
- Rust handles what it does best (fast in-memory compute, purpose-built algorithms)
- Clear separation of concerns
- Operational simplicity (Postgres for durability, Rust for performance)

## Deployment Options

1. **Sidecar**: Separate Rust binary, connects to Postgres over localhost
2. **Embedded**: Rust as library via napi-rs (Node.js) or PyO3 (Python)
3. **Standalone**: Single binary (embedded Postgres via libpq, Rust does everything)

Start with sidecar (simplest). Embedded is for later optimization.

## Open Questions

- What's the optimal sync frequency? (every insert, batched every second, pull-on-query?)
- Should Rust cache embeddings in Postgres to speed startup? (separate table)
- How do we handle Postgres connection loss? (queue writes in Rust, replay on reconnect?)
- Can we run multiple Rust sidecars against one Postgres (for scale/redundancy)?
- What's the memory budget for the in-memory graph? (bounded by available RAM)

## Related Decisions

- [triples-atomic]: Storage schema design (what tables, what indexes)
- [topology-embeddings]: Which algorithms to implement in Rust
- [graph-vector-duality]: How vector and graph indexes interact
- [rust-engine]: Overall engine architecture (this is a key piece)
