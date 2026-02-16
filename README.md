# valence-engine

The embeddable knowledge substrate for AI agents — and the network protocol for how knowledge travels between minds.

**Two layers, one vision:**
- **valence-engine**: The portable brain (embeddable Rust + PostgreSQL, runs anywhere)
- **Valence network**: The nervous system (federation protocol for trust-gated knowledge sharing)

Not a database. Not a vector store. A knowledge substrate that assembles context, computes confidence from topology, and shares capability through trust.

## What This Repo Is

This is the design space for valence-engine. We're using the structure of the ideas to organize the ideas — dogfooding the concept before writing code.

Every document has confidence scores. Open questions are tracked. Settled ideas are marked. **The graph of concepts is the product.**

## Core Thesis

> Stop treating the context window as a transcript. Treat it as an assembled worldview.

Instead of growing conversation history → compaction → loss, each inference call gets a freshly assembled context: the last few messages for thread continuity, plus exactly the knowledge that's relevant right now, surfaced by a multi-dimensional fusion query across an epistemic graph.

## Core Principles

1. **Triples as Atoms**: Knowledge decomposes to `(subject, relationship, object)` — not beliefs, not claims. Everything is triples: facts, taxonomy, provenance, confidence. The graph is self-describing.

2. **Three-Layer Architecture**: Triples (atomic facts) → Sources (provenance, never merged) → Summaries (LLM rendering at read boundary). Not three tables — TWO stored layers (triples + sources) plus on-demand rendering. Dedup at L1, preserve provenance at L2, render at L3. Summaries never hit storage.

3. **Topology-Derived Embeddings**: Vector embeddings generated from graph structure alone (spectral, random walks, message passing). No external model. No API dependency. Embeddings refine as graph grows.

4. **Graph-Vector Duality**: Graph = explicit structure (hard edges). Vector = implicit similarity (soft gradients). Two views of one system. Retrieval is hybrid: vectors find neighborhood, graph traversal finds precision.

5. **Complete Knowledge Loop**: LLM at boundary only (NL→triples on write, triples→NL on read). Engine is deterministic graph ops. Loop: LLM builds graph → topology generates embeddings → embeddings power retrieval → retrieval feeds LLM. Self-reinforcing.

6. **Cold/Warm Split**: Deterministic core (insert, link, retrieve, decay, evict, cluster) + inference enrichment (synthesis, labeling, curation). System works without inference at lower resolution.

7. **Inference as Training Loop**: Every query reshapes the substrate. No inner model, no weights. The LLM is purely at the boundary. Usage IS the training process.

8. **Value per Token**: Primary optimization metric. Maximize useful work per token consumed through precise context assembly, lazy compute, and tool mediation.

9. **Graceful Degradation**: Full function offline, without API, on slow hardware — just with lower resolution. Inference makes it richer but is never required.

10. **Bounded Memory**: Entire dataset is an LRU cache with hard boundary. Eviction IS forgetting. What matters gets retrieved, refreshed, and stays resident.

11. **Stigmergy + Progressive Summarization**: Structure emerges from use. Organization is continuous, proportional to value, and focused where it matters most.

12. **Dynamic Confidence**: Confidence dimensions are not stored metadata — they are computed from graph topology at query time. Source reliability, corroboration, temporal freshness, internal consistency, domain applicability — all emerge from structural analysis. Confidence is contextual: same triple can be high-confidence in one query context, low in another.

## Architecture: PostgreSQL + Rust Sidecar

**Decision (Feb 15, 2026)**: Split persistence from compute.

### PostgreSQL: Durable Triple Store
- Triple table with SPO/POS/OSP indexes (3 covering indexes for any query pattern)
- Source provenance (sessions, timestamps, metadata)
- Supersession chains (temporal validity)
- **What it's good at**: ACID transactions, durability, operational maturity

### Rust Sidecar: Compute Engine
- In-memory adjacency lists (O(1) graph traversal)
- Topology embeddings (spectral/walks/message-passing)
- HNSW vector index (ANN search)
- Dynamic confidence computation
- Fusion queries (vector + graph + confidence in one pass)
- **What it's good at**: Fast in-memory compute, predictable latency

**Implementation**: Build on [petgraph](https://docs.rs/petgraph/) with triple semantics layered on top.

## The Product: Engine + Network

This repo is **valence-engine** (the embeddable brain). The complete vision includes **Valence network** (the nervous system).

### valence-engine: The Brain
Portable, embeddable knowledge substrate. Runs:
- As sidecar (standalone process)
- Embedded (napi-rs for Node.js, PyO3 for Python)
- In browser (WASM)
- On edge devices (mobile, IoT)
- Inside future LLMs (structured memory layer)

Every agent/model/person gets their own engine instance with:
- Own knowledge graph (triples, sources, provenance)
- Own topology embeddings (computed from local graph)
- Own dynamic confidence (context-dependent, query-time)
- Own memory boundary (LRU eviction, user-controlled)

**Sovereign by design**: No shared state, no central authority, full user control.

### Valence Network: The Nervous System
Federation protocol connecting engine instances. Enables:
- **DIDs**: Decentralized identity (every engine has a DID)
- **Trust propagation**: Reputation flows through network based on corroboration quality
- **Consent-gated sharing**: Knowledge shares only with explicit permission
- **Verification**: Cryptographic provenance, tamper-evident logs
- **Capability sharing**: Models, skills, compute, trust scores — not just beliefs

**The protocol shares capability, not just information.**

What flows through the network:
- Beliefs (triples with provenance)
- Trust scores (reputation metadata)
- Boundary models (trained on usage data)
- Training data (for collective model improvement)
- Compute (delegate topology embeddings to trusted peers)

## Three Self-Closing Loops

The system improves itself through use via three reinforcing loops:

### Loop 1: Graph Builds Vectors (Topology Embeddings)
- Graph structure → topology analysis → vector embeddings
- No external model needed once graph is dense
- Progression: LLM embeddings (bootstrap) → hybrid → topology-only
- **Scaffolding falls away**: $$ API costs → $0

### Loop 2: Usage Builds Structure (Stigmergy)
- Queries reshape the graph (access patterns → structure)
- Frequently co-retrieved nodes cluster
- Hot paths strengthen, cold paths decay
- **The graph IS the cache**: Structure encodes usage patterns

### Loop 3: System Trains Its Own Boundary
- LLM at boundary generates training data (every NL↔triples call)
- Fine-tune small local model (1-3B params, MLX/ONNX)
- Replace LLM with local model (95%+ of calls)
- Progression: LLM everywhere → fine-tuned model → specialized encoder
- **Scaffolding falls away**: 10-50ms latency, $0 cost, fully offline

Each loop starts with expensive scaffolding, uses it to generate structure/data, builds cheaper replacement, then scaffolding falls away.

## Budget-Bounded Operations

Every operation has a budget (time, hops, results). Good-enough beats perfect when the read boundary is fuzzy.

Strategies:
- **Early termination**: Beam search on graph, stop when marginal relevance declines
- **Tiered retrieval**: Vector first → graph walk → confidence (return if good enough)
- **Materialized neighborhoods**: Pre-compute k-hop neighborhoods for hot nodes
- **Bloom filters on paths**: Fast reachability checks before BFS/DFS
- **Adaptive budgets**: Learn optimal budgets per domain from usage

Responsive > Complete. The LLM works with what it gets. 50ms with good results beats 2s with perfect results.

## Structure

```
docs/
  concepts/        — Core ideas, each as a node with confidence + status
  architecture/    — System design, layers, interfaces
  research/        — Competitive landscape, papers, prior art
  questions/       — Open questions that need resolution
  decisions/       — Settled decisions with reasoning chains
graph/
  NODES.md         — Index of all concept nodes
  EDGES.md         — Relationships between concepts
```

## Status

**Phase**: Design complete — ready for implementation (Feb 15, 2026)  
**Confidence**: 0.95 on core architecture  
**Origin**: Conversations between Chris Jacobs and Claude, Feb 2026  

**Latest**: Feb 15 2026 (v4) — Complete vision established:
- PostgreSQL + Rust sidecar architecture (persistence vs compute split)
- Budget-bounded operations (good-enough retrieval, responsive > complete)
- Self-training boundary models (system trains its own LLM replacement)
- Engine + network product decomposition (brain vs nervous system)
- Network flows capability, not just information (models, compute, trust)
- Emergence through composition (build pieces, let vision emerge)
- Three self-closing loops (graph→vectors, usage→structure, system→boundary)

## The Problem This Solves

**Current AI memory systems** have three core problems:

### 1. Accumulation Without Consolidation
- Query "Chris's preferences" → returns 15 overlapping similar beliefs
- No automatic deduplication (same insight from 5 sessions = 5 separate beliefs)
- Single-inference beliefs propagate as high-confidence truth
- Retrieval returns mush (redundant, overlapping, unstructured)

**Solution**: Three-layer architecture (triples → sources → summaries)
- All observations decompose to ~4 unique triples (dedup at L1)
- Each triple has 3-5 independent sources (corroboration at L2)
- Retrieval returns 1-2 clean summaries (LLM rendering at L3)

### 2. Expensive External Dependencies
- Vector embeddings require external API (cost scales with data)
- LLM boundary for decomposition/recomposition (every query costs $$)
- Can't run offline, can't preserve privacy

**Solution**: Self-closing loops
- Topology-derived embeddings (graph structure generates vectors, no API)
- Self-training boundary models (system generates training data, trains local models)
- Scaffolding falls away (LLM → fine-tuned 1-3B model → specialized encoder)

### 3. Isolated Knowledge Islands
- Every agent rebuilds the world from scratch
- No cross-agent corroboration, no shared learning
- Trust is binary (trusted source vs untrusted), not granular

**Solution**: Federation network
- Engines share triples with consent-gating and trust scoring
- Corroboration across nodes increases confidence
- Network shares capability (models, compute, skills), not just information
- Reputation emerges from accuracy (good sharers gain influence)

## Design Philosophy: Emergence Through Composition

**The vision emerges through iteration, not specification.**

Build pieces, use them, let friction reveal what's missing, build better pieces. Each piece built surfaces what the next piece needs to be.

Key insights emerged through use:
- **Beliefs → Triples**: Friction (can't query relationships) → Built (triple decomposition) → Emerged (graph structure)
- **External embeddings → Topology**: Friction (API costs, privacy) → Built (topology embeddings) → Emerged (zero-cost, offline-capable)
- **Inference everywhere → Cold/Warm split**: Friction (expensive, opaque) → Built (deterministic core + optional enrichment) → Emerged (graceful degradation)
- **Local engine → Network**: Friction (isolated knowledge) → Built (federation protocol) → Emerging (distributed cognition at scale)

**You design the pieces right and let emergence happen.**

The network at 1000 engines will exhibit patterns you can't design at 2 engines. That's the point.

## Lineage

Draws from:
- **Valence** — epistemic knowledge substrate (beliefs, confidence, federation) and the hygiene problems we discovered at scale (redundancy, poor consolidation)
- **pg_infinity** — Rust multi-dimensional search with adapter fusion (inspiration for bounded-budget fusion queries)
- **Bob/postgresql-singularity** — 5-dimensional memory (spatial, temporal, semantic, lexical, relational)
- **Stigmergy + progressive summarization** — structure emerges from use, organization is continuous
- **Graph topology research** — spectral embeddings, DeepWalk/Node2Vec, message passing (topology → vectors without learned weights)
- **petgraph** — Rust graph library (foundation for sidecar compute engine)
