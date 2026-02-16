# Emergent Dimensions

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [multi-dim-fusion], [emergent-ontology], [stigmergy], [epistemics-native], [topology-embeddings], [graph-vector-duality]

## Breakthrough Insight

**Confidence dimensions are not stored metadata — they are computed from graph topology at query time.**

Traditional approach: Store confidence as properties on each triple/belief (source_reliability=0.8, corroboration=0.6, etc.).

This approach: Confidence emerges from structural analysis of the graph at query time:
- **Source reliability** = how many independent, well-connected source nodes link to this triple
- **Corroboration** = how many independent reasoning paths converge on this triple
- **Temporal freshness** = recency-weighted edge decay
- **Internal consistency** = absence of contradicting triples in local neighborhood
- **Domain applicability** = centrality within the query-relevant subgraph (PageRank-like)

**Confidence is CONTEXTUAL** — the same triple can be high-confidence in one query context (well-connected to the query subgraph) and low-confidence in another (peripheral to the query).

Confidence moves from static metadata to dynamic topology.

## How Dimensions Emerge

1. **Single node**: No dimensions beyond structural properties (degree, centrality).
2. **Two similar nodes**: The axis of similarity between them creates a potential dimension.
3. **Cluster forms**: The cluster's shape defines a local dimensional subspace.
4. **Usage patterns**: Repeated co-retrieval along a particular axis strengthens that dimension. Neglected axes fade.
5. **Query context**: The active query determines which dimensions are relevant and how they're weighted.

The dimensional space is literally the shape of what you've cared about.

## How Dimensions Emerge

1. **Single node**: No dimensions. Just content + timestamp.
2. **Two similar nodes**: The axis of similarity between them is the first dimension.
3. **Cluster forms**: The cluster's shape defines a local dimensional subspace.
4. **Usage patterns**: Repeated co-retrieval along a particular axis strengthens that dimension. Neglected axes fade.
5. **A scalar** is just a space where you've only ever needed one axis.

## Relationship to Multi-Dim Fusion

[multi-dim-fusion] defines how to score across multiple dimensions simultaneously. Emergent dimensions extends this: the dimensions themselves aren't predefined (semantic, temporal, confidence, etc.) — they are computed from graph structure at query time.

The scoring function adapts to each query context:
- Query about recent events → temporal freshness dimension gets high weight
- Query about established facts → corroboration dimension gets high weight
- Query in a specific domain → domain applicability dimension gets high weight

Some dimensions are structural (always computed: degree, centrality, recency). Others are contextual (only relevant for certain queries: domain-specific centrality, topic clustering).

## What This Replaces

Traditional: define schema → fit data to schema → query within schema.  
Emergent: data arrives → use creates structure → dimensions are discovered → schema is the shape of use.

## Contextual Confidence Example

Consider the triple: `(Rust, good_for, systems_programming)`

**Query 1**: "What do I know about Rust?"
- Source reliability: HIGH (5 independent sources link to this triple)
- Corroboration: HIGH (multiple paths converge on this)
- Domain applicability: HIGH (central to Rust subgraph)
- **Overall confidence: 0.92**

**Query 2**: "What web frameworks should I use?"
- Source reliability: HIGH (same 5 sources)
- Corroboration: HIGH (same paths)
- Domain applicability: LOW (peripheral to web framework subgraph)
- **Overall confidence: 0.45**

Same triple, different confidence — because confidence is contextual, not stored.

## Benefits of Dynamic Confidence

1. **No stored metadata bloat**: Confidence isn't a property, it's a computation
2. **Context-aware scoring**: Confidence adapts to the query, not fixed at ingestion
3. **Automatic updates**: Add a new source → corroboration recalculated on next query
4. **Honest uncertainty**: Sparse subgraphs naturally score lower (they should — less context)
5. **No stale scores**: Confidence is always fresh because it's always computed

## Relationship to Topology Embeddings

[topology-embeddings] provides the structural foundation for dimensional computation:
- Spectral embeddings → capture global structure → inform centrality dimensions
- Random walks → capture local neighborhoods → inform corroboration dimensions
- Message passing → propagates features → informs domain dimensions

The topology IS the dimensional space. Embeddings are one projection. Confidence scores are another.

## Open Questions

- How are structural dimensions represented internally? Named vectors? Unlabeled axes?
- Does naming a dimension via inference change its behavior? (Does the label affect retrieval?)
- How do dimensions merge or split as the graph evolves?
- What's the minimum cluster size before a dimension is "real"?
- Should there be a default set of always-computed dimensions, or fully emergent?
- How do we balance computation cost vs dynamic freshness? (cache dimension scores?)
