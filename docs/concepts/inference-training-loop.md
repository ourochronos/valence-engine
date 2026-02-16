# Inference as Training Loop

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [cold-warm-split], [stigmergy], [lazy-compute], [knowledge-lifecycle]

## Idea

Every query reshapes the substrate. There is no inner model, no weights, no gradient descent. The LLM is purely at the boundary — interpreting queries in, interpreting results out. The "training" happens through deterministic structural changes driven by use.

## How Use Reshapes the Substrate

### Retrieval → Reinforcement
- A belief that gets retrieved has its confidence refreshed
- Its last_accessed timestamp updates
- Its retrieval_count increments
- In the bounded memory model, this makes it more likely to stay resident

### Co-Retrieval → Linking
- Beliefs retrieved together in the same context assembly get implicit connection weights strengthened
- This is how the graph self-organizes — frequently co-occurring knowledge naturally clusters

### Neglect → Decay
- Beliefs that stop being retrieved have confidence slowly decay
- In bounded memory, they become eviction candidates
- The system "forgets" what it doesn't use, naturally

### Contradiction → Tension
- When a new belief contradicts an existing one, a tension edge is created
- Future retrievals surface the tension, prompting resolution
- Resolution (supersession) strengthens the substrate's coherence

### Pattern Recognition → Synthesis
- When multiple beliefs cluster around a concept and get repeatedly co-retrieved, synthesis triggers (warm engine)
- A new L4 belief is created that captures the pattern
- This is emergent understanding, not programmed knowledge

## No Inner Model

Traditional ML: data → model training → model weights → inference
This engine: data → use-driven structure → retrieval → reshaping through use

There are no weights to tune. There is no model to train. The substrate IS the model. Its structure IS the weights. Usage IS the training process.

## LLM at the Boundary Only

The LLM's role:
- **Inbound**: Parse natural language queries into structured retrieval requests
- **Outbound**: Synthesize retrieved beliefs into natural language responses
- **Warm operations**: Generate labels, suggest connections, detect patterns

The LLM never sees the raw substrate. It doesn't manage the graph. It doesn't decide what decays or what links. Those are deterministic, use-driven processes.

## Why This Matters

1. **Inspectability**: Every change to the substrate is traceable to a query or ingestion event
2. **Predictability**: No black-box training process. You can reason about why the system behaved a certain way.
3. **Control**: The user's usage patterns shape the system, not an opaque optimization function
4. **Honesty**: The system can't hallucinate structure that isn't grounded in actual use
5. **Efficiency**: No separate training phase. The system improves as it's used, at zero extra cost.

## Stigmergy Connection

This is stigmergy at the epistemological level. Just as ants leave pheromone trails that guide future ants, queries leave structural traces (links, confidence updates, access patterns) that guide future queries.

The environment (the substrate) mediates coordination between queries across time.

## Open Questions

- Is there a floor to confidence decay? (Or can beliefs reach confidence=0 and become purely structural)
- Should there be a "batch training mode" where historical usage logs reshape the substrate in bulk?
- How do we handle cold starts? (New user has no usage history to drive structure)
- Can this training process be federated? (Usage patterns from Node A influence Node B's structure)
