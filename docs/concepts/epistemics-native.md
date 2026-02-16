# Epistemics-Native

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [multi-dim-fusion], [knowledge-lifecycle], [federation], [pekb], [emergent-dimensions], [three-layer-architecture]

## Idea

The engine natively understands that knowledge has quality, not just content. Confidence, provenance, contradictions, corroboration, and temporal validity are first-class dimensions that affect retrieval — not metadata bolted onto documents.

## What This Means

A belief with 0.95 confidence that's been corroborated by three independent sources and has no active tensions is *fundamentally different* from a belief with 0.5 confidence inferred from a single conversation. In every existing system, they're just two rows with the same retrieval weight. In this engine, they're treated differently at every level.

## Epistemic Primitives

- **Triple**: Atomic fact `(subject, relationship, object)` — the fundamental unit (see [triples-atomic])
- **Source**: Provenance record linking a triple to where it came from (see [three-layer-architecture])
- **Confidence**: NOT stored metadata — computed from graph topology at query time (see [emergent-dimensions])
  - Source reliability = independent, well-connected sources
  - Corroboration = independent reasoning paths to the same triple
  - Temporal freshness = recency-weighted decay
  - Internal consistency = absence of contradicting triples in local neighborhood
  - Domain applicability = centrality within query-relevant subgraph
- **Tension**: An explicit contradiction between two triples
- **Supersession**: A triple replacing an older triple, with reasoning chain preserved via sources

## Relationship to PEKBs

Proper Epistemic Knowledge Bases (formal AI research): syntactic knowledge bases using multi-agent epistemic logic for nested belief representation. "Agent A believes that Agent B knows X." This is the formal foundation for federation — when Valence instances share beliefs, the receiving node has beliefs *about* the source's beliefs.

## What Nobody Else Does

- Mem0: stores facts. No confidence. No contradictions.
- Zep: builds a knowledge graph. No epistemic dimensions.
- Letta: agent manages its own memory. No structural epistemics.
- OpenClaw memory-core: markdown files. No quality model at all.

Epistemic-native means the engine *reasons about what it knows*, not just *retrieves what it stored*.

## Open Questions

- How do we make epistemic dimensions useful to the LLM without overwhelming the context? (e.g., do we show confidence scores, or just rank by them?)
- Should the LLM be able to explicitly set confidence, or should it always be computed by the engine?
- How do we bootstrap confidence for new beliefs? (cold start problem)
