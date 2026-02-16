# Multi-Dimensional Fusion

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [context-assembly], [knowledge-graph], [epistemics-native], [rust-engine], [emergent-dimensions]

## Idea

A single query simultaneously searches across multiple dimensions — semantic similarity, graph relationships, confidence scores, temporal validity, corroboration status — and fuses results into one ranked set. Not separate queries stitched together. One unified scoring pass.

## Dimensions

1. **Semantic**: Vector similarity between query embedding and belief/triple embeddings
2. **Graph**: Traversal distance and relationship type from active entities
3. **Confidence**: Computed dynamically from topology (see [emergent-dimensions]) — source reliability, corroboration paths, consistency, domain centrality
4. **Temporal**: Superseded beliefs deprioritized; recent observations get recency boost (edge decay)
5. **Corroboration**: Number of independent reasoning paths to the triple (computed from source topology)
6. **Tension**: Contradicted beliefs surfaced *with* the contradiction, not hidden

**Note**: Dimensions 3-5 are NOT stored metadata — they are computed at query time from graph structure. This makes them contextual: the same triple can score differently in different query contexts.

## Lineage

- **pg_infinity**: Adapter routing + result fusion across pgvector, AGE, FTS, PostGIS
- **Bob**: 5-dimensional memory (spatial, temporal, semantic, lexical, relational)
- **Neither**: Both did fusion by combining separate query results. This does fusion in one pass.

## Why One Pass Matters

Separate queries + merge = O(n * dimensions) with post-hoc score normalization that's inherently lossy. A unified scoring function that considers all dimensions simultaneously produces better results because dimensions can inform each other:
- A low-similarity belief that's directly connected in the graph should rank higher
- A high-similarity belief with 0.3 confidence should rank lower than moderate-similarity at 0.9 confidence
- A perfectly relevant belief that's been superseded should surface the superseding belief instead

## Scoring Function

Weighted combination with configurable weights:
```
score = w_semantic * semantic_sim(query, triple)
      + w_graph * graph_proximity(active_entities, triple)
      + w_confidence * compute_confidence(triple, query_context)  // dynamic
      + w_recency * recency_decay(sources)  // from source timestamps
      + w_corroboration * count_independent_paths(triple)  // topology
      - w_tension * tension_penalty(triple)  // or: surface with tension flag
```

**Key insight**: `compute_confidence()` is NOT a lookup — it's a computation over the local graph topology in the context of the current query. Source reliability, corroboration, internal consistency, domain applicability — all computed fresh.

Weights can be context-dependent. A "verify this fact" query might weight confidence and corroboration higher. An "explore this topic" query might weight semantic similarity and graph breadth higher.

## Emergent Dimensions

The dimensions listed above (semantic, graph, confidence, temporal, corroboration, tension) are predefined starting points. [emergent-dimensions] extends this: the dimensions themselves are emergent, not just the fusion across them. As usage patterns create new structural axes, the scoring function adapts — new dimensions appear, unused dimensions fade. The dimensional space is the shape of what the user has cared about.

## Open Questions

- How do we tune the default weights? Learning from user behavior, or manual?
- Should the scoring function be pluggable/configurable per deployment?
- How does this interact with the lazy compute model — beliefs at L0 don't have embeddings yet?
- What's the performance target? <50ms for the full fusion pass?
