# Cold/Warm Engine Split

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [lazy-compute], [epistemics-native], [context-assembly], [graceful-degradation]

## Idea

The engine has two conceptual layers: a **cold engine** (deterministic core) and a **warm engine** (inference enrichment). The cold engine is the product. The warm engine is the experience. The system works without inference at lower resolution.

## Cold Engine (Deterministic Core)

Pure data operations with zero inference:
- **Insert**: Store belief, compute hash, assign ID
- **Link**: Create graph edges based on co-occurrence, explicit connections
- **Retrieve**: Multi-dimensional fusion query (vector similarity, graph traversal, confidence weighting)
- **Decay**: Time-based confidence reduction, access-based freshness
- **Evict**: LRU-based removal when memory bounds are reached
- **Cluster**: Group similar beliefs based on embedding proximity

All of these are deterministic, inspectable, and repeatable. No LLM calls. No inference. Pure computation.

## Warm Engine (Inference Enrichment)

Inference-driven enhancements that make the system richer:
- **Synthesis**: Generate L4 insights from L3 belief clusters
- **Labeling**: Assign semantic labels to clusters, derive entity types
- **Dimension Naming**: Name custom confidence dimensions based on domain
- **Curation**: Suggest supersessions, detect tensions, merge candidates
- **Intent Recognition**: Understand why a query is being made
- **Context Optimization**: Tune which knowledge to surface for this specific query

These operations use LLM reasoning to improve quality, but they're not required for function.

## Why This Split Matters

1. **Graceful Degradation**: System works offline, with no API key, on slow hardware — just with lower resolution
2. **Cost Control**: Inference budget is independent of system function. You can tune warm operations based on available budget.
3. **Transparency**: Cold operations are fully inspectable and debuggable. Warm operations are best-effort enhancements.
4. **Trust**: The deterministic core can be audited, proven correct, guaranteed to behave predictably.

## The Product/Experience Distinction

- **Cold engine is the product**: This is what you ship, what runs reliably, what scales predictably
- **Warm engine is the experience**: This is what makes it feel magical, what learns over time, what gets better with use

You sell the cold engine. The warm engine sells itself through use.

## Relationship to Lazy Compute

The cold/warm split reinforces lazy compute:
- Cold operations: always lazy (embedding on retrieval, not ingestion)
- Warm operations: even more lazy (synthesis only when patterns are established, not on every new belief)

Inference follows demonstrated value, not speculation.

## Open Questions

- What's the minimum viable cold engine? (What can we ship without any warm features?)
- Should warm operations be user-configurable? (e.g., disable synthesis to save costs)
- Can warm operations be batched/deferred? (e.g., run synthesis overnight when LLM calls are cheap)
- How do we communicate to users which features are cold vs warm?
