# Working Set

**Status**: exploring  
**Confidence**: 0.70  
**Connections**: [context-assembly], [decay-model], [intermediary]

## Idea

A compact representation of the active conceptual threads in a conversation. Not the full history — the live state. Updated between turns. Carries thread context without token cost of full message history.

## What It Contains

- Active topics being discussed (with recency weighting)
- Open questions / unresolved threads
- Decisions made this session
- Entities currently in focus
- The "shape" of the conversation — where we've been, where we're heading

## How It's Maintained

Between each turn:
1. Threads that resolved → drop or compress to a decision record
2. Threads that are active → strengthen
3. New threads that opened → add
4. Threads that were active but the conversation moved away from → weaken (but don't drop — they might return)

## Representation

Open question: is this a text summary, a structured object, or a subgraph?

- **Text summary**: Compact, LLM-readable, but lossy and hard to update incrementally
- **Structured object**: Machine-readable, precise updates, but requires serialization for the LLM
- **Subgraph**: A live view into the knowledge graph filtered to "what's active now" — most aligned with the architecture but most complex

Likely: a hybrid. A small structured object that references graph nodes, serialized to text for the LLM context.

## Relationship to Conversation History

The working set *replaces* most of what conversation history provides. Instead of "here are 30 messages that establish context," the working set says "here's what we're thinking about right now, here's what we decided, here are the open threads."

The last 2-3 messages provide conversational continuity. The working set provides conceptual continuity. Together they replace the full history.

## Open Questions

- Who maintains the working set — the engine, the LLM, or both?
- How large can it get before it becomes its own token problem?
- Should there be a "parking lot" for threads that went dormant but might return?
- How does the working set transfer across sessions? (Next day, same project)
