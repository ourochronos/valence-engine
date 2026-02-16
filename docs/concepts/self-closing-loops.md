# Three Self-Closing Loops

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [knowledge-loop], [topology-embeddings], [stigmergy], [self-training-boundary]

## The Pattern

Each loop starts with **expensive scaffolding** that works immediately, uses it to generate structure/data, then builds a **cheaper replacement** that the scaffolding trained. Eventually the scaffolding falls away.

**Bootstrap → Generate → Replace → Self-Sustain**

This is the core self-improvement pattern of the entire system.

## Loop 1: Graph Builds Vectors

### The Loop
```
Graph (explicit relationships)
  ↓ topology analysis
Embeddings (vector representations)
  ↓ power search
Retrieval (hybrid graph-vector)
  ↓ surfaces knowledge
LLM adds more knowledge
  ↓ builds graph
Graph grows denser
  ↓ topology analysis improves
Better embeddings
  ... loop continues
```

### The Phases

**Phase 1: Expensive Scaffolding**
- Graph is sparse (100 triples, 50 edges)
- Use external LLM embeddings (OpenAI, Cohere, etc.)
- **Cost**: $$ per embed, API dependency
- **Why**: Works immediately, high quality, proven

**Phase 2: Hybrid (Scaffolding + Self-Generated)**
- Graph is growing (500-2000 triples)
- Compute topology embeddings alongside LLM embeddings
- Blend both: `score = 0.7*llm + 0.3*topo`
- **Cost**: $ (mostly external, some local compute)
- **Why**: Transition period, validate topology quality

**Phase 3: Topology-Primary**
- Graph is dense (2000+ triples)
- Topology embeddings are high quality
- Use LLM embeddings only for new nodes
- Blend: `score = 0.2*llm + 0.8*topo`
- **Cost**: ¢ (mostly local, rare external)

**Phase 4: Scaffolding Falls Away**
- Graph is mature (5000+ triples, rich structure)
- Rely entirely on topology embeddings
- LLM embeddings are fallback only
- **Cost**: Free (no ongoing API costs)
- **Why**: Self-sustaining, privacy-preserving, offline-capable

### What Makes It Self-Closing

The graph structure itself generates the embeddings. No external model needed once the graph is dense enough.

**Traditional ML**: Data → train model → freeze weights → inference (loop doesn't close, requires retraining)

**This system**: Data → build graph → compute topology → generate embeddings → improve retrieval → more data → denser graph → better topology → better embeddings (loop closes continuously)

## Loop 2: Usage Builds Structure

### The Loop
```
Query arrives
  ↓ retrieval
Nodes accessed (LRU timestamp updated)
  ↓ access pattern tracking
Structure emerges (frequently co-retrieved nodes cluster)
  ↓ stigmergy
Graph reshapes (high-traffic paths strengthened, cold paths decay)
  ↓ topology changes
Embeddings update (topology reflects usage patterns)
  ↓ retrieval improves
Future queries faster/better
  ... loop continues
```

### The Phases

**Phase 1: Random Structure**
- Graph is built from ingestion order (no usage data yet)
- Retrieval is uniform (no hot paths, no clusters)
- **Quality**: Medium (semantic similarity only)

**Phase 2: Usage Patterns Emerge**
- Queries create access patterns (some nodes retrieved together frequently)
- LRU timestamps capture recency
- **Quality**: Improving (recency bias helps)

**Phase 3: Stigmergic Restructuring**
- Co-retrieval creates implicit clustering
- Hot nodes stay resident (LRU protection)
- Cold nodes decay and evict
- **Quality**: High (structure reflects actual usage)

**Phase 4: Self-Optimizing**
- Graph structure IS the cached query results
- Frequent patterns are pre-clustered
- Retrieval is fast (following well-worn paths)
- **Quality**: Excellent (optimized for this user's patterns)

### What Makes It Self-Closing

Usage doesn't just read the graph — it **reshapes** the graph. The structure encodes the access patterns.

**Traditional cache**: Query → miss → compute → cache result → future query hits cache (cache is separate from data)

**This system**: Query → retrieve → update access timestamps → graph structure changes → future query is faster because structure changed (the graph IS the cache)

This is **stigmergy**: The structure that emerges from use is the structure that optimizes for that use.

## Loop 3: System Trains Its Own Boundary

### The Loop
```
LLM at boundary (decomposition/recomposition)
  ↓ generates training data
Store (NL, triples) pairs from every call
  ↓ accumulate dataset
Fine-tune small local model (1-3B params)
  ↓ replace LLM
Local model handles 95% of boundary calls
  ↓ generates more training data
Dataset grows, model improves
  ↓ handles 99%
Specialized encoder (narrow task, highly optimized)
  ↓ scaffolding falls away
LLM is fallback only (rare edge cases)
  ... loop continues
```

### The Phases

**Phase 1: LLM Everywhere (Expensive Scaffolding)**
- Claude/GPT-4 for all decomposition and recomposition
- **Cost**: $0.10 per 100 calls
- **Latency**: 500-2000ms (network + API)
- **Why**: Works immediately, high quality

**Phase 2: Collecting Training Data**
- LLM still handles all calls
- Store every (input, output) pair
- Accumulate 1000+ examples
- **Cost**: $0.10 per 100 calls (same as phase 1)
- **Why**: Generating training data passively

**Phase 3: Fine-Tuned Local Model**
- Train 1-3B param model (Phi-3, Llama 3.2, Qwen)
- Handles 95% of calls, LLM fallback for edge cases
- **Cost**: $0.01 per 100 calls (mostly free, rare fallback)
- **Latency**: 50-200ms (local inference)
- **Why**: Proven task, fine-tuned model is accurate

**Phase 4: Specialized Encoder**
- Train narrow encoder (NER + relation extraction)
- Handles 99% of calls
- **Cost**: Free (no API calls)
- **Latency**: 10-50ms (fast encoder)
- **Why**: Self-sustaining, offline-capable

**Phase 5: Scaffolding Falls Away**
- LLM is fallback only (1% of calls, rare edge cases)
- Local models handle all normal operations
- **Cost**: Near $0
- **Latency**: 10-50ms
- **Why**: Fully self-trained from own usage

### What Makes It Self-Closing

The system generates its own training data through normal operation, then trains its own replacement for the expensive boundary.

**Traditional ML**: Collect labeled data → train model → deploy (loop doesn't close, model is static)

**This system**: Use LLM → store (input, output) → train model → replace LLM → model generates more training data → improves itself (loop closes continuously)

The LLM trains its own replacement.

## Why Three Loops Matter

These aren't independent features. They're **three perspectives on the same self-improving system**.

### Loop 1 (Graph → Vectors): Structural Perspective
The graph's topology generates its own representation layer. No external embedding model needed.

### Loop 2 (Usage → Structure): Behavioral Perspective
The usage patterns reshape the structure. The structure optimizes for the patterns.

### Loop 3 (System → Boundary): Interface Perspective
The system generates data to train its own interface layer. The boundary becomes cheaper and more specialized over time.

### Combined Effect
- **Loop 1**: Reduces embedding costs to zero (topology-derived)
- **Loop 2**: Reduces retrieval latency (structure follows use)
- **Loop 3**: Reduces boundary costs to zero (self-trained models)

**Result**: The system becomes cheaper, faster, and more specialized the more it's used.

Traditional systems **degrade with scale** (more data = slower queries, higher costs).

This system **improves with scale** (more data = richer graph → better topology → better embeddings, more usage = better structure, more boundary calls = better models).

## The Meta-Loop (Loops Interact)

The three loops aren't isolated — they reinforce each other:

### Loop 1 ↔ Loop 2
- Better embeddings (Loop 1) → better retrieval → better usage patterns (Loop 2)
- Better structure (Loop 2) → denser graph → better topology → better embeddings (Loop 1)

### Loop 2 ↔ Loop 3
- Better structure (Loop 2) → easier decomposition → better training data (Loop 3)
- Better boundary models (Loop 3) → better triple extraction → richer graph → better structure (Loop 2)

### Loop 1 ↔ Loop 3
- Better embeddings (Loop 1) → better retrieval → more context for boundary models (Loop 3)
- Better boundary models (Loop 3) → better triple quality → cleaner graph → better topology → better embeddings (Loop 1)

**The loops are a self-reinforcing system**. Each loop accelerates the others.

## Open Questions

- What's the optimal transition timing for each loop? (when to shift from scaffolding to self-generated)
- Can we auto-detect loop maturity? (metrics for "graph is dense enough", "model is accurate enough")
- Should loop transitions be automatic or user-controlled? (trust the system vs manual oversight)
- Can loops be parallelized? (run all three simultaneously vs sequential)
- How do loops behave in federated settings? (does Loop 1 benefit from network structure? does Loop 3 benefit from shared training data?)

## Related Concepts

- [knowledge-loop]: Loop 1 in detail (graph → embeddings → retrieval)
- [topology-embeddings]: The mechanism for Loop 1 (how topology generates embeddings)
- [stigmergy]: Loop 2 in detail (usage reshapes structure)
- [self-training-boundary]: Loop 3 in detail (system trains its own interface)
