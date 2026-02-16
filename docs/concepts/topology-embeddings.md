# Topology-Derived Embeddings

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [graph-vector-duality], [knowledge-loop], [stigmergy], [lazy-compute]

## Breakthrough Insight

Vector embeddings can be derived from graph topology alone, without an external language model.

No pretrained model. No training corpus. No GPU required. The graph's structure generates its own vector space.

## Three Proven Approaches

### 1. Spectral Embedding
Pure linear algebra. Compute eigenvectors of the graph Laplacian.

```
L = D - A  (Laplacian = Degree matrix - Adjacency matrix)
Compute eigenvectors of L
Use first k eigenvectors as embeddings
```

**Why it works**: Eigenvectors capture the natural frequencies of the topology. Nodes with similar structural roles cluster in vector space, even if not directly connected.

**Properties**:
- Deterministic
- No parameters to learn
- Fast (sparse matrix operations)
- Embeddings encode graph structure (spectral properties)

### 2. Random Walks (DeepWalk / Node2Vec)
Sample the graph via random walks. Compress walk statistics into vectors.

```
For each node:
  Generate random walks starting from that node
  Treat walks as "sentences", nodes as "words"
  Train skip-gram model on walks (compresses context)
  Node embedding = skip-gram vector
```

**Why it works**: Nodes that appear in similar walk contexts get similar embeddings. This captures local neighborhood similarity.

**Properties**:
- Stochastic but converges
- Parameters: walk length, walks per node, window size
- Very efficient (no full graph pass needed)
- Embeddings encode local structure

### 3. Message Passing (Graph Neural Networks without learned weights)
Iterative neighborhood aggregation. Each node's vector encodes its k-hop neighborhood.

```
Initialize: random or feature-based vectors
For k iterations:
  For each node:
    Aggregate neighbor vectors (mean/max/sum)
    Update node vector based on aggregation
Final vectors = embeddings
```

**Why it works**: Even without learned parameters, mean aggregation produces useful embeddings. Nodes with similar neighborhoods get similar vectors.

**Properties**:
- Deterministic with fixed aggregation
- Stochastic with random init (but stable after convergence)
- Captures k-hop structure (k = iteration count)
- Can incorporate node features if available

## Why This Is Huge

Current approach: 
- Ingest text → call external embedding API (BERT, Ada, etc.) → get vectors → store in pgvector
- **Dependency**: external model, API costs, model drift, embedding compatibility across federation

New approach:
- Build graph from triples → compute topology embeddings from structure alone → use for retrieval
- **Dependency**: none. Pure deterministic computation on the graph you already have.

## Stigmergy in Vector Space

As the graph grows, embeddings automatically refine:
- Add edge → shifts local neighborhood → vectors update
- More triples → denser structure → better embeddings
- Usage builds structure → structure generates geometry → geometry powers retrieval

The loop closes. No external model needed.

## Bootstrap Problem & Solution

**Problem**: Early graph is sparse. Topology embeddings from 100 nodes are low-quality.

**Solution**: Use traditional LLM embeddings (BERT/Ada) as scaffolding:
1. Cold start: use LLM embeddings for retrieval
2. Graph densifies through use
3. At threshold density (~1000 triples, ~500 edges), compute topology embeddings
4. Hybrid: blend LLM + topology embeddings with shifting weights
5. At maturity, rely primarily on topology embeddings
6. LLM embeddings become fallback for new nodes

The scaffolding falls away as the structure self-supports.

## Performance Characteristics

| Method | Compute Cost | Quality | Stability |
|--------|--------------|---------|-----------|
| Spectral | O(n²) sparse | Good | Perfect (deterministic) |
| Random Walk | O(walks × length) | Very Good | High (converges) |
| Message Passing | O(k × edges) | Good | High (converges) |

For a 10K node graph:
- Spectral: ~1-2s on CPU
- Random Walk: ~500ms on CPU
- Message Passing: ~200ms on CPU (k=5)

All orders of magnitude faster than external API calls.

## Relationship to Graph-Vector Duality

When embeddings come from topology:
- Vector space is a **projection** of graph space, not a parallel system
- Proximity in vector space is grounded in structural relationships
- The two views are aspects of one system, not bolted-together systems

This is the fundamental insight: graph and vector aren't separate. Vector space is the continuous shadow of the discrete graph.

## Federation Implications

Current problem: Different nodes use different embedding models → incompatible vector spaces → can't compare embeddings across federation.

With topology embeddings:
- Each node generates embeddings from its own graph
- Shared triples get re-embedded locally
- No embedding compatibility problem (you never share vectors)
- Text-based similarity (BM25) handles cross-node search

## Open Questions

- Which method is best for which graph characteristics? (dense vs sparse, small vs large)
- Can we blend all three methods? (spectral for global, walks for local, message passing for features)
- How do we handle dynamic graphs? (recompute embeddings on every insert, or batch updates?)
- What's the minimum graph density for useful topology embeddings? (100 nodes? 1000?)
- Should we expose embedding method as user choice, or auto-select based on graph properties?
