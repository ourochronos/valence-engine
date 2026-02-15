# Intent Capture

**Status**: exploring  
**Confidence**: 0.75  
**Connections**: [tool-mediation], [procedural-memory], [context-assembly]

## Idea

Wrap tool calls with structured intent and reasoning. The system learns *why* actions are taken, not just *what* actions occur. Over time, this enables pattern recognition, short-circuiting, and reasoning reconstruction.

## What Gets Captured

- **Intent**: What the agent is trying to accomplish
- **Reasoning**: Why this specific tool/approach was chosen
- **Decision context**: What broader decision this informs
- **Outcome**: Did it resolve the intent? (captured after)

## Value Chain

1. **Immediate**: Better context assembly next time similar intent arises
2. **Short-term**: Post-hoc reasoning reconstruction when asked "why did you do that?"
3. **Medium-term**: Pattern recognition — common intent chains become procedural memory
4. **Long-term**: Preemptive resolution — engine recognizes intent and resolves without tool call

## Emergent Intent Taxonomy

Intent types should emerge from use, not be predefined. But natural categories observed so far:
- resolve_uncertainty — "I need to know X"
- verify_belief — "is X still true?"
- execute_action — "do X in the world"
- explore_topic — "what's out there about X?"
- compare_options — "X vs Y for purpose Z"

Whether these become formal types or stay soft labels — open question.

## Procedural Memory Connection

Intent chains that repeat become learned procedures:
- "resolve_uncertainty about library" → web_search → web_fetch → extract_facts → compare
- Over time, the engine recognizes the chain and can suggest or preemptively execute steps

This is procedural memory — learned behaviors, not just facts.

## Open Questions

- How do we capture intent without adding overhead to every tool call?
- Is intent inferred from context, or does the LLM explicitly declare it?
- How do we distinguish between similar intents that should share patterns vs genuinely different intents?
- Does intent capture happen in the engine layer or the LLM layer?
