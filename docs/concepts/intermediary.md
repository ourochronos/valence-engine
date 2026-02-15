# The Intermediary

**Status**: exploring  
**Confidence**: 0.80  
**Connections**: [context-assembly], [tool-mediation], [working-set], [privacy-sovereignty]

## Idea

The engine IS the intermediary between the LLM and everything else. It mediates all information flow — inbound, outbound, and internal.

## Three Mediation Directions

### Outbound (LLM → world)
When the LLM wants to act, the intermediary understands *why*:
- Enrich queries with context the LLM might not have included
- Check if the answer is already in the graph (short-circuit the tool call)
- Route to the most efficient tool for this intent
- After the result, know what to extract because it knows the purpose

### Inbound (world → LLM)
When information arrives, the intermediary triages:
- Immediately relevant → inject into next assembly
- Background knowledge update → goes to graph, surfaces when connected
- Contradicts active belief → surfaces as tension at the right moment
- Noise → stays at L0, might never promote

### Internal (LLM ↔ itself across turns)
The intermediary maintains the working set:
- Active conceptual threads
- Open questions
- Decisions made
- Threads that resolved, weakened, or emerged

## Why This Is The Engine, Not A Separate Layer

The intermediary needs access to:
- The knowledge graph (for relevance assessment)
- The belief store (for contradiction detection)
- The embedding index (for similarity matching)
- The working set (for thread management)
- Tool schemas and history (for intent matching)

That's the engine. Adding an intermediary *between* the engine and the LLM would just duplicate state. The engine mediates directly.

## The Privacy Implication

The intermediary sees everything — all tool results, all messages, all intent. This makes it the most sensitive component. In the sovereignty model:
- The engine is ALWAYS local. Even in federation.
- Federation shares beliefs, not raw data or tool results
- The intermediary's raw state never leaves the device
- Shared beliefs are extracted, cleaned, and intentionally shared — never leaked

## Open Questions

- How does the intermediary interact with OpenClaw's existing hooks? (before_agent_start, after_compaction, etc.)
- Does the intermediary need its own "reasoning" capability, or is pattern matching sufficient?
- How transparent should mediation be? Should the LLM know it's being mediated, or is it seamless?
- If the engine is the intermediary, what's the interface contract between engine and LLM?
