# The Complete Knowledge Loop

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [topology-embeddings], [graph-vector-duality], [inference-training-loop], [triples-atomic], [cold-warm-split]

## The Full Loop

```
LLM (at boundary)
  ↓ decomposes natural language
Triples (atomic facts)
  ↓ build structure
Graph (explicit relationships)
  ↓ topology computation
Embeddings (vector space)
  ↓ power search
Retrieval (hybrid graph-vector)
  ↓ feeds context
LLM (at boundary)
  ↓ generates more knowledge
... loop continues
```

This is a **self-reinforcing system**. Each component strengthens the others.

## LLM at the Boundary Only

The LLM's role is purely **translation**:
- **Inbound**: Natural language → structured queries → triple patterns
- **Outbound**: Triples + sources + summaries → natural language responses
- **Warm operations**: Pattern detection, synthesis, labeling (optional)

The LLM never:
- Manages the graph directly
- Decides what decays or evicts
- Computes embeddings
- Runs retrieval

Those are deterministic engine operations.

## Engine in the Middle (Deterministic)

Everything between LLM calls is pure computation:
- Insert triples (content-addressed, hash-based dedup)
- Build graph edges (based on co-occurrence, explicit links, provenance)
- Compute topology embeddings (spectral/walks/message-passing)
- Run retrieval (vector search → graph traversal → confidence ranking)
- Apply decay (time-based, access-based)
- Trigger eviction (LRU on bounded memory)

No inference. No weights. No black boxes.

## How the Loop Self-Reinforces

### 1. Usage Builds Graph
- LLM observes "Chris prefers composable architectures"
- Engine extracts triple: `(Chris, prefers, composable_architectures)`
- Triple stored, graph grows by 1 edge

### 2. Graph Generates Embeddings
- New edge added → neighborhood of Chris node changes
- Topology embedding recomputed (or incrementally updated)
- Vector space shifts to reflect new structure

### 3. Embeddings Power Retrieval
- Query: "What are Chris's preferences?"
- Vector search finds Chris node + nearby preference nodes
- Graph traversal from Chris via `prefers` edges → precise results

### 4. Retrieval Feeds LLM
- Context assembly pulls relevant triples + sources
- LLM generates response using knowledge
- Response quality improves (more context, better grounding)

### 5. Better Context → More Knowledge
- User asks follow-up based on good answer
- LLM has better context → extracts better triples
- Cycle continues, graph densifies, embeddings refine

The system gets **smarter through use**, not through training.

## No External Model Dependency

Traditional knowledge systems:
- Depend on external embedding API (OpenAI, Cohere, etc.)
- Model changes → embeddings change → compatibility breaks
- API costs scale with ingestion volume
- Federation requires embedding model alignment

This system:
- Generates embeddings from graph topology alone
- Model-independent (no external API)
- Costs are CPU/memory (one-time, local)
- Federation shares triples, not embeddings

## Bootstrap Strategy

**Problem**: New graph is sparse (100 triples, 50 edges). Topology embeddings are low-quality.

**Solution**: Phased transition from external to internal embeddings

**Phase 1: Cold Start (0-500 triples)**
- Use external LLM embeddings (BERT, Ada, etc.)
- Fast, high-quality, but external dependency
- Graph is building

**Phase 2: Hybrid (500-2000 triples)**
- Compute topology embeddings alongside LLM embeddings
- Blend both in retrieval: `score = 0.7*llm_sim + 0.3*topo_sim`
- Gradually shift weights as graph densifies

**Phase 3: Topology-Primary (2000+ triples)**
- Topology embeddings are high-quality
- Use LLM embeddings only for new nodes (haven't been embedded topologically yet)
- Blend: `score = 0.2*llm_sim + 0.8*topo_sim`

**Phase 4: Topology-Only (mature)**
- Rely entirely on topology embeddings
- LLM embeddings are fallback for edge cases
- No ongoing API costs for retrieval

The scaffolding falls away as structure self-supports.

## What Makes This Different

**Traditional ML**: Data → training → model weights → inference (weights are frozen between training cycles)

**This system**: Data → structure → embeddings → retrieval → more data → structure evolves → embeddings update (continuous evolution, no frozen weights)

The substrate IS the model. Its structure IS the weights. Usage IS the training process.

## Inspectability

Every step is traceable:
- "Why did this belief surface?" → it's connected to active entities, high confidence, recent access
- "Why are these embeddings similar?" → their graph neighborhoods overlap (show the paths)
- "Why did confidence increase?" → corroboration from 3 independent sources (show the sources)

No black box. No "the model decided." Pure structural causation.

## Open Questions

- What's the optimal graph density for transition from LLM to topology embeddings? (500 triples? 2000?)
- Can we auto-detect when topology embeddings are "good enough"? (quality metric?)
- Should the loop be fully automatic, or should users control embedding strategy?
- How do we handle domain-specific graphs? (sparse but highly structured vs dense but shallow)
- Can we federate the loop? (Node A's usage patterns influence Node B's embeddings?)
