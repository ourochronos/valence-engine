# Tool Mediation

**Status**: exploring  
**Confidence**: 0.80  
**Connections**: [context-assembly], [intent-capture], [knowledge-lifecycle], [stigmergy]

## Idea

Tools are bidirectional knowledge conduits, not fire-and-forget actions. The engine mediates all tool interactions, extracting knowledge from results and wrapping calls with intent.

## Three Roles

### 1. Knowledge Pull (preemptive enrichment)
The engine sees that the conversation trajectory connects to artifacts it can fetch. It preemptively surfaces relevant information in the context assembly — not because the agent asked, but because the graph connects current discussion to those artifacts.

### 2. Knowledge Push (extraction from results)
When tools return results, the engine:
- Extracts beliefs/entities from the result
- Connects to active conversation entities
- Assesses against existing knowledge (corroborate/contradict/novel)
- Lets raw output fade from transcript while knowledge persists in graph

### 3. Hygiene Triggers (micro-organization)
Every tool interaction is an organization opportunity:
- Search returns similar results → flag merge candidates
- Fetch contradicts a belief → create tension
- Exec confirms something → boost confidence
- Search returns nothing → note knowledge gap

## The Wrapper

Tool calls carry structured intent:
```
intent: what are you trying to accomplish
reasoning: why this tool helps
decision_context: what decision this informs
```

This doesn't add tool calls — it makes the system smarter about tool calls over time.

## Efficiency Learning Loop

- Early: many exploratory tool calls, each captured with intent
- Over time: frequently resolved intents get cached in graph
- Later: engine short-circuits — "you already have a high-confidence belief about this"
- Result: agent gets faster and cheaper the longer it runs

## Tool Surface Reduction

LLM-facing tools collapse from 56 to ~3-5:
- **think** — explicit capture ("remember this")
- **recall** — explicit query ("what do I know about X?")
- **connect** — explicit graph creation ("these are related")

Everything else (create, supersede, archive, tension management) becomes engine internals driven by use patterns.

## Open Questions

- How does the wrapper interact with tool schemas? Is it metadata alongside the call, or does it change the call format?
- How aggressive should preemptive enrichment be? Too much = noise, too little = missed connections
- Should the efficiency loop be explicit ("I've seen this before") or transparent?
- How do we handle tools that have side effects (exec, message send) vs read-only tools differently?
