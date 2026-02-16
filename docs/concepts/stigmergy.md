# Stigmergy

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [progressive-summarization], [emergent-ontology], [lazy-compute], [decay-model], [deterministic-core], [bounded-memory], [merge-model]

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

## Concrete Mechanism: The Deterministic Core

The stigmergic model now has a concrete implementation through [deterministic-core]:
- **Co-retrieval counting** creates links — when two nodes are pulled together, the engine records the structural association
- **Retrieval refreshes** nodes — use IS the reinforcement signal, implemented as LRU score updates
- **LRU eviction** ([bounded-memory]) is the forgetting mechanism — unused traces fade and are eventually removed
- **Clustering** ([merge-model]) is the consolidation mechanism — frequently co-retrieved nodes merge into cluster nodes

The deterministic core IS stigmergy made concrete. Every operation (refresh, link, decay, evict, cluster) is a deterministic trace-based mechanism. No inference needed.

## Key Property

The system gets *better organized* the more it's used. Not through entropy accumulation, but through use-driven refinement. A frequently-accessed knowledge area becomes well-structured naturally. A rarely-accessed area stays rough but costs nothing.
