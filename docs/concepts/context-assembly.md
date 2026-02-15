# Context Assembly

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [working-set], [multi-dim-fusion], [tool-mediation], [decay-model]

## Idea

Each LLM inference call receives a freshly assembled context rather than an accumulated conversation history. The context window is not a transcript — it's a constructed worldview for this moment.

## Components of an Assembled Context

1. **Immediate thread** — Last 2-3 messages. Conversational momentum and continuity.
2. **Relevant knowledge** — Beliefs from the graph scored by multi-dimensional fusion against the current message.
3. **Active entities** — Focused subgraph of people, projects, concepts live in this conversation.
4. **Tensions** — Contradictions between what was just said and what's believed.
5. **Confidence profile** — Signal to the LLM about what's well-known vs uncertain.

## Why This Matters

- Conversations never overflow — no growing log
- Context is always relevant — no wasted tokens on stale messages
- Knowledge compounds across conversations — the graph carries it
- The LLM gets better input — less noise, better reasoning

## Analogy

When a human gets a new message, they don't re-read the entire conversation. They have the last thing said, their knowledge of the topic, and when something from earlier becomes relevant, it surfaces naturally because it's *connected* to what's being discussed now.

## Open Questions

- How much conversation history is optimal? 2-3 messages, or adaptive based on conversation density?
- Should the assembly include a "conversation summary" or is the working set sufficient?
- How do we handle the first turn of a new session? (Cold start assembly)
