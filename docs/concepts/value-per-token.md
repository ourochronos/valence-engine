# Value Per Token (Primary Metric)

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [lazy-compute], [context-assembly], [cold-warm-split], [tool-mediation]

## Idea

Value per token is the primary design metric for the entire system. Every architectural decision should increase the ratio of useful information to tokens consumed.

## What This Means

Traditional metric: accuracy, recall, F1, latency
This system's metric: **How much useful work per token spent?**

Tokens are consumed in:
1. Context assembly (beliefs loaded into LLM context)
2. LLM inference (input + output tokens)
3. Tool results (returned data)
4. Warm engine operations (synthesis, labeling, etc.)

Value is produced by:
1. Correct decisions
2. Useful knowledge retrieved
3. Problems solved
4. Patterns learned

## Design Implications

### Context Precision
Load exactly what's needed via multi-dimensional fusion, not bulk dumps.
- Bad: Include all conversation history (tokens scale linearly with time)
- Good: Include last 2-3 messages + relevant beliefs (tokens stay constant)

### Non-Inference Solutions
Scripts, regex, git hooks, deterministic routing — wherever possible.
- Bad: Use LLM to parse timestamps from strings
- Good: Use `chrono` library (zero tokens)

### Tool Mediation
Extract knowledge from results; let raw output fade.
- Bad: Keep entire 50KB tool result in context for the rest of the session
- Good: Extract 3 key beliefs, discard raw result, keep beliefs in graph

### Lazy Compute
Process only what gets used.
- Bad: Embed every email at ingestion (1000 emails × 512 tokens = 512K tokens)
- Good: Embed on retrieval (10 emails retrieved × 512 tokens = 5K tokens)

### Local Inference for Cheap Tasks
Essentially free inference locally vs API costs.
- Bad: Call Opus to classify "is this a question?" (10K tokens/day = $150/month)
- Good: Run local Gemma 4B classifier (free after hardware)

## Feedback Loops That Improve Value/Token

1. **Usage statistics**: Track which beliefs get retrieved most → prioritize those in fusion
2. **Confidence updates**: High-confidence beliefs surface first → fewer tokens wasted on uncertain knowledge
3. **Intent learning**: Recognize common query patterns → short-circuit with cached insights
4. **Synthesis**: Combine 10 low-level beliefs into 1 high-level insight → 10× context efficiency

## The Goal: System Gets Cheaper Over Time

Most AI systems: more use = more cost
This system: more use = better structure = more efficient retrieval = lower cost per query

Early: exploratory queries, lots of tool calls, inefficient context
Later: established patterns, cached insights, precise retrieval
Long-term: most queries resolved from graph, minimal LLM usage

## Anti-Patterns to Avoid

1. **Verbose context**: Including full conversation history instead of working set
2. **Redundant retrieval**: Fetching the same belief multiple times in one session
3. **Low-confidence inclusion**: Loading beliefs with 0.3 confidence into high-stakes decisions
4. **Noise amplification**: Keeping raw tool results instead of extracted knowledge
5. **Batch processing**: Running inference on everything instead of lazy processing

## Measuring Value/Token

```
value_per_token = useful_actions_taken / total_tokens_consumed

useful_actions_taken:
  - Questions answered correctly
  - Decisions supported by evidence
  - Knowledge gaps filled
  - Patterns detected
  
total_tokens_consumed:
  - Input to LLM (context assembly)
  - Output from LLM (responses)
  - Tool results (before extraction)
  - Warm engine operations (synthesis, labeling)
```

Goal: maximize this ratio across the system's lifetime.

## Open Questions

- How do we measure "useful actions" objectively?
- Can we build value/token tracking into the engine itself?
- Should users see value/token metrics? ("This session: 0.73 useful actions per 1K tokens")
- How do we balance value/token with other metrics like latency and user experience?
- Is there a theoretical ceiling to value/token, or can it improve indefinitely?
