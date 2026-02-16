# Budget-Bounded Operations

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [lazy-compute], [value-per-token], [graceful-degradation], [context-assembly]

## Core Principle

**Every operation has a budget** — time, hops, results, tokens. When the budget is exhausted, return what you have. Good-enough beats perfect when the read boundary is fuzzy anyway.

Exact correctness is less valuable than responsive approximation when:
- Context assembly is inherently approximate (LLM will work with what you give it)
- The "complete answer" is unknowable (graph is evolving, confidence is contextual)
- User experience degrades with latency (200ms is acceptable, 2s is not)

## Budget Types

### Time Budget
- **Context assembly**: 50-100ms max
- **Graph traversal**: stop after N milliseconds, return partial results
- **Embedding computation**: lazy, interruptible (checkpoint and resume later)

### Hop Budget
- **Graph walk**: limit depth (e.g., 3 hops from query nodes)
- **Prevents**: Runaway traversal on dense graphs
- **Trade-off**: Might miss distant but relevant nodes

### Result Budget
- **Vector search**: top-k (e.g., 20 candidates)
- **Graph expansion**: max N nodes in working set
- **Prevents**: Context overflow (LLM has finite window)

### Token Budget
- **LLM calls**: track cumulative token usage, stop enrichment when budget hit
- **Warm operations**: synthesis, labeling can be deferred if budget is tight
- **User-controlled**: "spend up to $0.10 on this query" → auto-stops

## Strategies

### 1. Early Termination (Beam Search on Graph)
Start with vector search → top-k candidates. Expand outward via graph edges. Track marginal relevance of each expansion step.

**Stop when**:
- Marginal relevance declines (new nodes less relevant than current best)
- Budget exhausted (time, hops, result count)
- No new nodes discovered (reached graph boundary)

```
Iteration 1: 20 candidates, relevance score: 0.85
Iteration 2: 35 nodes (+15), relevance score: 0.80  ← still improving
Iteration 3: 48 nodes (+13), relevance score: 0.62  ← declining, STOP
```

Return 35 nodes from iteration 2.

### 2. Tiered Retrieval (Progressive Refinement)
Don't do everything at once. Do just enough, check if it's good enough, continue if needed.

**Tier 1**: Vector search only (fast, broad)
- If top result confidence ≥ 0.9 → return immediately

**Tier 2**: + Graph walk from top-5 (medium, precise)
- If aggregate confidence ≥ 0.85 → return

**Tier 3**: + Full confidence computation from topology (slow, thorough)
- Compute all dimensions, return best results

Most queries resolve at Tier 1 or 2. Tier 3 is rare.

### 3. Materialized Neighborhoods (Hot Nodes)
Some nodes are queried frequently ("Chris", "preferences", "architecture"). Pre-compute and cache their k-hop neighborhoods.

**On query**: Check if query node has materialized neighborhood → instant retrieval  
**On update**: Mark affected neighborhoods as dirty → recompute lazily

**Trade-off**: Memory vs speed. Materialize top 5-10% most-accessed nodes.

### 4. Bloom Filters on Paths
Graph traversal asks: "Is there a path from A to B within k hops?"

**Naive**: BFS/DFS traversal (expensive on dense graphs)  
**Optimized**: Bloom filter per node encoding reachable nodes within k hops

```rust
struct Node {
    id: String,
    reachable_2hop: BloomFilter,  // nodes within 2 hops
    reachable_4hop: BloomFilter,  // nodes within 4 hops
}
```

**Query**: "Can I reach B from A in 3 hops?"
- Check `A.reachable_2hop.contains(B)` → if yes, definitely within 2 (return true)
- Check `A.reachable_4hop.contains(B)` → if no, definitely not within 4 (return false)
- If uncertain (Bloom says maybe) → fall back to BFS

**Trade-off**: False positives possible (Bloom filter artifact) but no false negatives. Reduces traversals by 80-90% on typical graphs.

### 5. Confidence Approximation (Coarse → Fine)
Dynamic confidence from topology is expensive (requires analyzing local neighborhood, source diversity, temporal freshness).

**Coarse confidence** (cheap):
- Degree centrality (well-connected nodes are more confident)
- Recency (recently accessed nodes are more confident)
- Source count (more sources = higher confidence)

**Fine confidence** (expensive):
- Full dimensional analysis (corroboration, internal consistency, domain applicability)
- Requires graph analysis, path finding, neighborhood clustering

**Strategy**: Use coarse confidence for ranking candidates. Compute fine confidence only for top-10 before returning.

### 6. Adaptive Budgets (Learn from Usage)
Track query performance: latency, result quality (did user engage with results?), budget utilization.

**Adjust**:
- If queries consistently finish under budget → reduce budget (free up resources)
- If queries consistently hit budget with low-quality results → increase budget
- Per-domain budgets: technical queries get more hops, casual chat gets fewer

**Example**:
```
Domain: "architecture" → budget: 100ms, 5 hops, 50 results
Domain: "preferences" → budget: 50ms, 3 hops, 20 results
Domain: "unknown" → budget: 75ms, 4 hops, 30 results (default)
```

## Design Philosophy

### Fuzzy Read Boundary
The LLM is the consumer of retrieved knowledge. LLMs are:
- Robust to incomplete information (they work with what they get)
- Good at synthesizing from partial data
- Not harmed by missing the 47th-most-relevant fact

This means **perfect retrieval is not required**. Good-enough retrieval with 10x faster latency is better UX.

### Responsive > Complete
User types message → 200ms later, LLM starts generating.

**Bad**: Wait 2 seconds for exhaustive graph traversal → perfect context → LLM responds  
**Good**: Return top-30 most relevant nodes in 50ms → good context → LLM responds

The user experiences responsiveness. The LLM gets sufficient context. Win-win.

### Emergence Through Iteration
Budget-bounded operations don't mean "give up on quality." They mean "optimize for iteration."

**Single-pass exhaustive** (slow, complete, discourages exploration):
- User asks question
- System spends 3 seconds finding EVERYTHING
- Returns results
- User refines question (another 3 seconds...)

**Multi-pass budget-bounded** (fast, iterative, encourages exploration):
- User asks question
- System spends 50ms finding GOOD results
- Returns results
- User refines based on what they see (another 50ms)
- Conversation flows naturally

Three iterations of budget-bounded (150ms total) often yields better outcomes than one exhaustive pass (3000ms).

## Implementation

### Budget Tracking
```rust
struct QueryBudget {
    max_time_ms: u64,
    max_hops: usize,
    max_results: usize,
    start_time: Instant,
}

impl QueryBudget {
    fn is_exhausted(&self) -> bool {
        self.start_time.elapsed().as_millis() as u64 >= self.max_time_ms
    }
    
    fn can_expand(&self, current_hops: usize, current_results: usize) -> bool {
        current_hops < self.max_hops && 
        current_results < self.max_results &&
        !self.is_exhausted()
    }
}
```

### Beam Search with Budget
```rust
fn search_with_budget(graph: &TripleGraph, query: &Query, budget: QueryBudget) -> Vec<Node> {
    let mut beam = initial_candidates(query);  // vector search, top-k
    let mut best_score = beam.iter().map(|n| n.score).sum();
    
    for hop in 0..budget.max_hops {
        if budget.is_exhausted() { break; }
        
        beam = expand_beam(graph, &beam, budget.max_results);
        let current_score = beam.iter().map(|n| n.score).sum();
        
        // Early termination: marginal gain < 10%
        if (current_score - best_score) / best_score < 0.10 {
            break;
        }
        
        best_score = current_score;
    }
    
    beam
}
```

## Open Questions

- What are good default budget values? (domain-dependent? user-configurable?)
- How do we communicate partial results to users? ("Found 30 results in 50ms, stopped early")
- Should budgets be strict (hard stop) or soft (guidance with override)?
- Can we auto-tune budgets from query feedback? (measure quality vs latency trade-off)
- How do budget-bounded ops interact with federation? (multi-node queries need distributed budgets)

## Related Concepts

- [lazy-compute]: Budget-bounded is extreme laziness (do minimum viable work)
- [value-per-token]: Budget forces value maximization (can't waste tokens on low-value results)
- [graceful-degradation]: Budget-bounded enables graceful degradation (tight budget = lower resolution, not failure)
- [context-assembly]: Primary application (assemble context within budget)
