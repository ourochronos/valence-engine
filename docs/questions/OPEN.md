# Open Questions

Tracked questions that need resolution. Each links to the concept(s) it affects.

## Architecture

### Q1: Simple-first or paradigm-shift-first?
**Affects**: [rust-engine]  
Months of Rust work vs Python/TS prototype that proves context assembly. Could prototype the assembly logic in the current plugin, then port.

### Q2: Graph storage approach?
**Affects**: [rust-engine], [emergent-ontology]  
Purpose-built adjacency structures, petgraph, or lean on an existing embedded graph (Kuzu, CozoDB)? Chris said no Neo4j ever. Considering Rust sidecar.

### Q3: Embedded model distribution?
**Affects**: [rust-engine], [lazy-compute]  
Ship ~80MB BERT weights in binary, or download on first use (like QMD)? Tradeoff: install size vs first-run latency.

## Context Assembly

### Q4: Optimal conversation history length?
**Affects**: [context-assembly], [working-set]  
2-3 messages fixed, or adaptive based on conversation density? Dense technical discussions might need more immediate history.

### Q5: Working set representation?
**Affects**: [working-set]  
Text summary, structured object, or subgraph? Likely hybrid — structured object referencing graph nodes, serialized to text for LLM.

### Q6: Who maintains the working set?
**Affects**: [working-set], [intermediary]  
Engine alone, LLM alone, or collaborative? The LLM understands conversational nuance. The engine understands the graph.

## Tools & Intent

### Q7: How is intent captured without overhead?
**Affects**: [intent-capture], [tool-mediation]  
Inferred from context by the engine? Explicitly declared by the LLM in tool call metadata? Some hybrid?

### Q8: How aggressive should preemptive enrichment be?
**Affects**: [tool-mediation], [context-assembly]  
Too much = noise and wasted compute. Too little = missed connections. Need a relevance threshold.

## Ontology

### Q9: Any seed types needed, or fully emergent?
**Affects**: [emergent-ontology]  
Fully emergent is philosophically clean but cold start is hard. Maybe lightweight seed types that can be overridden?

### Q10: Entity resolution across names?
**Affects**: [emergent-ontology]  
"Chris", "Chris Jacobs", "zonk1024" → same entity. How? Embedding similarity of contexts? Explicit linkage? Co-occurrence?

## Privacy & Federation

### Q11: Transitive provenance in federation?
**Affects**: [federation], [privacy-sovereignty]  
If Node A shares a belief derived from Node B's shared belief — how much of the chain is visible?

### Q12: Belief revocation after sharing?
**Affects**: [federation], [privacy-sovereignty]  
Can you "unshare" a belief? The receiving node already has it. Revocation = removing trust signal, not deleting data?

## Performance

### Q13: Context assembly latency target?
**Affects**: [rust-engine], [multi-dim-fusion]  
<50ms? <100ms? What's acceptable between user message and LLM inference start?

### Q14: L0 query performance at scale?
**Affects**: [lazy-compute], [knowledge-lifecycle]  
When a query touches large volumes of L0 data, how do we avoid scanning everything? Cheap indexes on L0?

## Bounded Memory & New Architecture

### Q15: What's the right LRU boundary size?
**Affects**: [bounded-memory]  
10K nodes? Adaptive based on available storage? Per-domain budgets? The boundary is the single most impactful configuration parameter.

### Q16: Cluster representative selection
**Affects**: [merge-model]  
Most-retrieved member, or most-connected? Does inference-generated synthesis become the representative? What happens when the current representative gets superseded?

### Q17: Eviction of linked nodes
**Affects**: [bounded-memory], [merge-model]  
If a node is scheduled for eviction but has active links to resident nodes, how much protection does that give? Is link-count protection linear, logarithmic, or threshold-based?

### Q18: Pattern-significant storage format
**Affects**: [curation]  
What's the minimal statistical fingerprint for noise data (email frequency, sender clusters, timing) that captures patterns without storing content? Histogram? Time series? Count vector?

### Q19: Federation + LRU interaction
**Affects**: [bounded-memory], [federation]  
Federated beliefs enter local LRU and compete for residency? Or separate tier? If separate, does that break the bounded guarantee?

### Q20: Cold start symmetry breaking
**Affects**: [bounded-memory], [deterministic-core]  
With no usage history, everything has equal priority. What breaks the symmetry? Ingestion order? Source weighting? Explicit seeding?

### Q21: Emergent dimension naming
**Affects**: [emergent-dimensions]  
Structural dimensions are unnamed. Does naming them via inference change behavior? Does a label make a dimension "stickier" or bias retrieval toward it?
