# Deterministic Core

**Status**: exploring  
**Confidence**: 0.85  
**Connections**: [rust-engine], [stigmergy], [lazy-compute], [curation], [bounded-memory]

## Idea

Inference is the training loop. Every query reshapes the substrate through deterministic mechanisms — retrieval refreshes, co-retrieval creates links, neglect causes decay. No inner model, no weights, no gradient descent. The LLM's role is purely at the boundary: interpreting queries in, interpreting results out. The engine itself is deterministic, inspectable, and honest.

## Cold Engine vs Warm Engine

The system splits into two layers:

### Cold Engine (the product)
Deterministic operations only:
- **Insert**: Add a node to the substrate
- **Link**: Create edges between nodes (from co-retrieval or explicit action)
- **Retrieve**: Fetch nodes by query, refresh their scores
- **Decay**: Deprioritize unused nodes over time
- **Evict**: Remove lowest-scoring nodes at the LRU boundary
- **Cluster**: Group co-retrieved nodes into cluster nodes

This layer has no inference dependency. It runs on its own. It's the thing you ship.

### Warm Engine (the experience)
Inference-enriched operations layered on top:
- **Synthesis**: Generate summaries across clusters
- **Labeling**: Name emergent clusters or dimensions
- **Dimension naming**: Give human-readable labels to structural axes
- **Enrichment**: Add context, implications, or connections via LLM

The warm engine makes the experience better but the cold engine doesn't depend on it. Graceful degradation is structural — if inference is unavailable, the system still works.

## Why Deterministic Matters

- **Inspectable**: Every state change has a traceable cause (this query refreshed these nodes, these co-retrievals created this link)
- **Honest**: The system doesn't hallucinate structure. Structure emerges from actual use.
- **Reproducible**: Same sequence of operations → same state. Debugging is straightforward.
- **Cheap**: Deterministic bookkeeping costs orders of magnitude less than inference.

## Open Questions

- Where exactly is the boundary between cold and warm? Is entity extraction cold (regex/rules) or warm (LLM)?
- How do we version the deterministic state for rollback/audit?
- Should the cold engine expose a formal spec (state machine, event log)?
