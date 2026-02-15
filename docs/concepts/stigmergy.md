# Stigmergy

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [progressive-summarization], [emergent-ontology], [lazy-compute], [decay-model]

## Idea

Indirect coordination through environmental traces. Knowledge self-organizes through use rather than through upfront schema or explicit organization steps.

## Application to Knowledge

- Every piece of data enters at the same low level (L0)
- Retrieval is the reinforcement signal — what gets used gets stronger
- What never gets retrieved fades (not deleted — decayed)
- Organization happens as a side effect of normal use
- The most-used knowledge becomes the most-refined
- Organization depth correlates with utility

## The Hygiene Function

Every query is a micro-organization pass. When you already have a pointer to data:
1. Look for similar items nearby
2. Organize a little (merge candidates, detect tensions)
3. Leave the remainders
4. No separate "organize the KB" step needed

This means the system never needs a batch cleanup job. Maintenance is continuous, proportional to use, and focused where it matters most.

## What This Replaces

Traditional approaches: scheduled maintenance, batch reindexing, explicit curation
Stigmergic approach: continuous, use-driven, emergent

## Key Property

The system gets *better organized* the more it's used. Not through entropy accumulation, but through use-driven refinement. A frequently-accessed knowledge area becomes well-structured naturally. A rarely-accessed area stays rough but costs nothing.
