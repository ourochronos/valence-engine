# Bounded Memory

**Status**: exploring  
**Confidence**: 0.80  
**Connections**: [decay-model], [stigmergy], [knowledge-lifecycle], [working-set]

## Idea

The entire dataset is an LRU cache with a hard boundary (configurable, e.g. 10K nodes). Everything competes for residency. The system can never grow past the boundary. This solves unbounded growth without cleanup jobs.

## Eviction Score

Eviction is multi-dimensional, not just recency:
- **Recency**: When was this node last retrieved?
- **Retrieval frequency**: How often has it been pulled?
- **Connection count**: How well-linked is it in the graph?
- **Explicit pins**: User or agent can pin nodes to protect them from eviction.

These factors combine into an eviction score. Lowest-scoring nodes are candidates for removal when the boundary is hit.

## Eviction IS Forgetting

This is correct by construction. If something mattered, it got retrieved. Retrieval refreshed it. Refreshing kept it resident. Things that nobody ever asks about drift to the bottom and eventually get evicted. No separate garbage collection, no cleanup jobs, no "organize the KB" pass. The boundary enforces hygiene structurally.

## What This Replaces

Traditional: unbounded growth → periodic cleanup → batch deletion → hope you didn't delete something important.  
Bounded memory: fixed capacity → continuous competition → organic forgetting → anything important self-preserves through use.

## Interaction with Decay

[decay-model] handles deprioritization within the resident set — decayed beliefs still exist but surface less. Bounded memory is the hard stop: when a node's combined score (including decay) drops low enough relative to the rest, it's evicted entirely. Decay feeds eviction scores.

## Open Questions

- What's the right boundary size? 10K nodes? Adaptive based on storage? Per-domain budgets?
- How much protection does link count give? A node with many active connections should be harder to evict.
- How do federated beliefs interact with the local LRU? Separate tier or same pool?
- What breaks the symmetry at cold start when everything has equal priority?
