# Epistemics-Native

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [multi-dim-fusion], [knowledge-lifecycle], [federation], [pekb]

## Idea

The engine natively understands that knowledge has quality, not just content. Confidence, provenance, contradictions, corroboration, and temporal validity are first-class dimensions that affect retrieval — not metadata bolted onto documents.

## What This Means

A belief with 0.95 confidence that's been corroborated by three independent sources and has no active tensions is *fundamentally different* from a belief with 0.5 confidence inferred from a single conversation. In every existing system, they're just two rows with the same retrieval weight. In this engine, they're treated differently at every level.

## Epistemic Primitives

- **Belief**: A statement with dimensional confidence (source reliability, temporal validity, corroboration count)
- **Tension**: An explicit contradiction between two beliefs
- **Supersession**: A belief replacing an older belief, with reasoning chain preserved
- **Corroboration**: Independent confirmation from different sources
- **Provenance**: Where a belief came from (which ingestion events, which sessions, which tool results)

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
