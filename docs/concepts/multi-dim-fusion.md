# Multi-Dimensional Fusion

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [context-assembly], [knowledge-graph], [epistemics-native], [rust-engine], [emergent-dimensions]

## Idea

A single query simultaneously searches across multiple dimensions — semantic similarity, graph relationships, confidence scores, temporal validity, corroboration status — and fuses results into one ranked set. Not separate queries stitched together. One unified scoring pass.

## Dimensions

1. **Semantic**: Vector similarity between query embedding and belief embeddings
2. **Graph**: Traversal distance and relationship type from active entities
3. **Confidence**: Higher-confidence beliefs rank higher (but aren't just filtered)
4. **Temporal**: Superseded beliefs deprioritized; recent beliefs get recency boost
5. **Corroboration**: Independently verified beliefs get ranking lift
6. **Tension**: Contradicted beliefs surfaced *with* the contradiction, not hidden

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
score = w_semantic * semantic_sim(query, belief)
      + w_graph * graph_proximity(active_entities, belief)
      + w_confidence * belief.confidence
      + w_recency * recency_decay(belief.timestamp)
      + w_corroboration * corroboration_boost(belief)
      - w_tension * tension_penalty(belief)  // or: surface with tension flag
```

Weights can be context-dependent. A "verify this fact" query might weight confidence and corroboration higher. An "explore this topic" query might weight semantic similarity and graph breadth higher.

## Emergent Dimensions

The dimensions listed above (semantic, graph, confidence, temporal, corroboration, tension) are predefined starting points. [emergent-dimensions] extends this: the dimensions themselves are emergent, not just the fusion across them. As usage patterns create new structural axes, the scoring function adapts — new dimensions appear, unused dimensions fade. The dimensional space is the shape of what the user has cared about.

## Open Questions

- How do we tune the default weights? Learning from user behavior, or manual?
- Should the scoring function be pluggable/configurable per deployment?
- How does this interact with the lazy compute model — beliefs at L0 don't have embeddings yet?
- What's the performance target? <50ms for the full fusion pass?
