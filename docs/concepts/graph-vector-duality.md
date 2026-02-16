# Graph-Vector Duality

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [topology-embeddings], [multi-dim-fusion], [knowledge-loop], [emergent-ontology]

## Idea

Graph space and vector space are two views of the same knowledge, not two separate systems.

- **Graph space**: Explicit structure, hard edges, traversable, provenance-preserving
- **Vector space**: Implicit similarity, soft gradients, discoverable, continuous

They inform each other bidirectionally.

## Graph → Vectors

Topology-derived embeddings make this direction explicit:
- Graph structure (edges, neighborhoods, paths) → spectral/walk/message-passing computation → vector embeddings
- Adding an edge shifts local neighborhoods → vectors update
- The vector space is a **projection** of the graph, not an independent construction

## Vectors → Graph

The reverse direction discovers structure:
- Nodes close in vector space but not connected in graph → candidate edges
- "These two beliefs embed near each other, maybe there's a relationship we haven't stated"
- Clustering in vector space suggests structural communities
- Anomalies in vector space (distant from cluster) highlight outliers or errors

## Retrieval as Hybrid Traversal

Query: "What do I know about composable architectures?"

**Vector search** (broad, fast):
- Finds neighborhood in embedding space
- Returns candidates: 50 nodes within distance threshold
- This is the "landing zone"

**Graph traversal** (precise, structured):
- From those 50 candidates, walk the graph
- Follow explicit relationships: `is_a`, `related_to`, `sourced_from`
- Filter by confidence, temporal validity, corroboration
- Return structured results with provenance

**Analogy**: Vectors get you to the right neighborhood. Graph gets you to the right house.

## Benefits of Unified View

When embeddings come from topology (not external models):
1. **Consistency**: Vector proximity is grounded in structural relationships, not statistical accidents
2. **Interpretability**: Similar embeddings = similar neighborhoods, not "BERT thought they were similar"
3. **Evolvability**: Graph changes → embeddings update automatically
4. **No drift**: External embedding models can change (model updates, API deprecation). Topology embeddings are stable.

## Two Aspects of One System

This is not:
- Graph database + vector database (two systems)
- Graph with vector index bolted on (one primary, one secondary)

This is:
- One knowledge substrate with two projections
- Discrete (graph) and continuous (vector) views of the same structure
- Operations in either space affect both

## Discovery Loop

1. LLM decomposes text into triples → builds graph
2. Graph topology generates embeddings → builds vector space
3. Vector search finds clusters → suggests new graph edges
4. Graph edges refine structure → embeddings update
5. Better embeddings → better retrieval → more graph building

The system self-organizes through this loop.

## What This Replaces

**Traditional RAG**:
- Text → embeddings (external model) → vector DB
- Retrieval = pure similarity search
- No structure, no provenance, no relationships

**GraphRAG** (Microsoft et al):
- Text → LLM extraction → knowledge graph
- Graph → external embeddings → store both
- Retrieval = query graph OR query vectors (two systems)

**This approach**:
- Text → triples → graph
- Graph → topology embeddings → vector space
- Retrieval = hybrid (vectors for neighborhood, graph for precision)
- One system, two projections

## Open Questions

- How do we tune the blend between vector and graph retrieval? (context-dependent weights?)
- Should vector space ever override graph structure? (e.g., embeddings suggest connection but graph says no)
- Can we visualize the duality? (graph layout driven by embeddings, or vice versa?)
- How does this interact with external knowledge? (federated triples that haven't been embedded locally yet)
- What's the performance profile? (graph traversal from 50 candidates vs pure vector search of 10K nodes)
