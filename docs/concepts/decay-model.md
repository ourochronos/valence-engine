# Decay Model

**Status**: exploring  
**Confidence**: 0.70  
**Connections**: [context-assembly], [working-set], [knowledge-lifecycle], [stigmergy]

## Idea

Information relevance decays based on structural properties, not just time. Superseded beliefs decay differently than unused beliefs. Conversation context decays through the working set, not through message truncation.

## Types of Decay

### Conversation Decay (within session)
- Last 2-3 messages: full fidelity
- Older messages: absorbed into working set threads
- Much older messages: only their extracted knowledge persists (in graph)
- The conversation transcript itself is NOT the persistence layer

### Belief Decay (across sessions)
- Active beliefs (frequently retrieved): stable or growing confidence
- Dormant beliefs (not retrieved recently): slow confidence decay
- Superseded beliefs: rapid deprioritization but retained in chain
- Contradicted beliefs: flagged but not automatically removed

### Entity Decay
- Frequently referenced entities: well-connected, strong nodes
- Rarely referenced entities: fade toward L0
- Entities only connected to decayed beliefs: candidates for pruning

### Thread Decay (in working set)
- Active thread: full weight in context assembly
- Dormant thread (last referenced 10+ turns ago): weakened but present
- Resolved thread: compressed to decision record, minimal weight
- Abandoned thread: fades from working set, knowledge absorbed into graph

## Key Principle

Decay in this system is not deletion. It's deprioritization in the fusion query. A decayed belief still exists. It just doesn't surface unless a query specifically reaches for it. This means nothing is truly lost — but the context assembly naturally focuses on what's alive and relevant.

## How This Replaces Compaction

Current model: grow context → hit limit → summarize (lossy) → lose detail
New model: context is assembled fresh each turn → no growth → no compaction needed → no loss

The "decay" is just the natural result of assembly: if it's not relevant to this turn, it's not in the context. It's still in the graph. It can come back.

## Open Questions

- What's the decay rate for different types? (Linear, exponential, stepped?)
- Should the engine proactively prune, or let decay be purely organic?
- How do we prevent important but infrequently accessed knowledge from decaying? (e.g., "Chris's birthday")
- Is there a "pin" mechanism for beliefs that should never decay?
