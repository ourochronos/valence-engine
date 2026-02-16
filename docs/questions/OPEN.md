# Open Questions

Tracked questions that need resolution. Each links to the concept(s) it affects.

## Architecture

### ~~Q1: Simple-first or paradigm-shift-first?~~ ✅ RESOLVED (Feb 15, 2026)
**Affects**: [rust-engine]  
**Decision**: Paradigm-shift-first. Build Rust + PostgreSQL sidecar architecture directly. The vision is clear enough to build confidently.

### ~~Q2: Graph storage approach?~~ ✅ RESOLVED (Feb 15, 2026)
**Affects**: [rust-engine], [emergent-ontology], [postgres-rust-architecture]  
**Decision**: PostgreSQL for durable triple store (SPO/POS/OSP indexes), Rust sidecar with petgraph for in-memory compute. Split: Postgres = persistence, Rust = compute.

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

## Belief Lifecycle & Confidence

### Q22: How do we prevent single-inference beliefs from propagating as truth?
**Affects**: [epistemics-native], [knowledge-lifecycle]  
KB shows this is a real problem: one conversation infers "Chris doesn't like frameworks" and it carries forward as established truth. Beliefs from single interactions should have LOW confidence. Confidence should only increase through independent corroboration (multiple sessions observing the same pattern). The 6D confidence model has a corroboration dimension but it's never updated after creation.

### Q23: What counts as independent corroboration?
**Affects**: [epistemics-native], [knowledge-lifecycle]  
Same user saying it twice in one session ≠ corroboration. Same pattern observed across 3+ independent sessions = corroboration. How do we track session independence? How do we prevent gaming?

### Q24: Belief deduplication strategy?
**Affects**: [knowledge-lifecycle], [bounded-memory]  
Current Valence has content_hash but never checks it for uniqueness. Options: (1) Exact hash check on create (fast, catches exact dupes), (2) Embedding similarity check before create (slow, catches fuzzy matches), (3) Periodic batch consolidation. On match, bump confidence + add corroboration instead of creating duplicate.

## Cold/Warm Split

### Q25: Minimum viable cold engine?
**Affects**: [cold-warm-split], [rust-engine]  
What's the smallest feature set we can ship without any warm features? Insert, retrieve, decay, evict? Or do we need basic synthesis?

### Q26: Warm operations user-configurable?
**Affects**: [cold-warm-split], [value-per-token]  
Should users be able to disable synthesis to save costs? Run labeling only on weekends? How granular should control be?

### Q27: Can warm operations be batched/deferred?
**Affects**: [cold-warm-split], [lazy-compute]  
Run synthesis overnight when LLM calls are cheap? Or does deferring break the feedback loop?

## Bounded Memory

### Q28: Right boundary sizes?
**Affects**: [bounded-memory]  
10K beliefs feels reasonable for personal use. What about teams? Enterprises? Should boundaries scale with available resources?

### Q29: Eviction communication?
**Affects**: [bounded-memory], [privacy-sovereignty]  
Do we tell users "You're at 92% capacity"? Suggest archiving? Auto-archive with notification? Silent eviction?

### Q30: Can archived beliefs be searched?
**Affects**: [bounded-memory], [knowledge-lifecycle]  
Are they truly offline, or do they stay in a "cold storage" tier that's searchable but slow?

## Value Metrics

### Q31: How do we measure "useful actions" objectively?
**Affects**: [value-per-token]  
Questions answered correctly, decisions supported by evidence, knowledge gaps filled, patterns detected — how do we track these automatically?

### Q32: Should users see value/token metrics?
**Affects**: [value-per-token]  
"This session: 0.73 useful actions per 1K tokens" — helpful feedback or confusing noise?

## Triples & Three-Layer Architecture (NEW - Feb 15, 2026)

### Q33: Triple granularity?
**Affects**: [triples-atomic], [three-layer-architecture]  
How atomic should triples be? Single assertion per triple, or can objects be complex structures? `(Chris, prefers, composable_architectures)` vs `(Chris, prefers, {type: architecture, property: composable})`?

### Q34: Temporal validity at which layer?
**Affects**: [triples-atomic], [three-layer-architecture]  
Do triples have temporal bounds, or only sources? Is `(Chris, prefers, X)` timeless with time-bounded sources, or is the triple itself temporal?

### Q35: Migration path from beliefs to triples?
**Affects**: [triples-atomic], [three-layer-architecture]  
Current Valence has ~800 beliefs. How do we decompose them into triples without losing information? Automated extraction, manual review, hybrid?

### Q36: Triple serialization for LLM context?
**Affects**: [triples-atomic], [context-assembly]  
How do we present triples to the LLM without overwhelming context? Show raw triples, always use summaries, or adaptive based on query type?

### Q37: Summary caching strategy?
**Affects**: [three-layer-architecture]  
Summaries are rendered on-demand, but should we cache them? For how long? Or always render fresh? What's the trade-off between computation cost and staleness?

## Topology Embeddings (NEW - Feb 15, 2026)

### Q38: Which topology method for which graphs?
**Affects**: [topology-embeddings], [graph-vector-duality]  
Spectral for dense graphs? Random walks for sparse? Message passing for feature-rich? Can we auto-select based on graph properties?

### Q39: Dynamic graph embedding updates?
**Affects**: [topology-embeddings], [lazy-compute]  
Recompute all embeddings on every triple insert (expensive), incremental updates (complex), or batch recompute (lazy but stale)?

### Q40: Minimum graph density threshold?
**Affects**: [topology-embeddings], [knowledge-loop]  
How many triples/edges before topology embeddings are "good enough"? 100 nodes? 1000? Can we measure embedding quality to auto-transition?

### Q41: Blend weights for hybrid embeddings?
**Affects**: [topology-embeddings], [knowledge-loop]  
During bootstrap phase, how do we blend LLM + topology embeddings? Fixed weights, adaptive based on graph density, learned from retrieval quality?

### Q42: Topology method as user choice or auto?
**Affects**: [topology-embeddings]  
Expose embedding method as configuration, or auto-select based on graph characteristics? Spectral vs DeepWalk vs message passing — user decision or engine decision?

## Graph-Vector Duality & Knowledge Loop (NEW - Feb 15, 2026)

### Q43: Vector-graph blend tuning?
**Affects**: [graph-vector-duality], [multi-dim-fusion]  
How do we balance vector search (broad) vs graph traversal (precise) in retrieval? Context-dependent weights, user preference, learned from feedback?

### Q44: Should vectors override graph?
**Affects**: [graph-vector-duality]  
If embeddings suggest two nodes are similar but no graph edge exists, should we create a candidate edge, ignore the signal, or surface as tension?

### Q45: Loop phase transition detection?
**Affects**: [knowledge-loop]  
How do we know when to transition from LLM embeddings to topology embeddings? Graph density metric, embedding quality metric, manual threshold?

### Q46: Domain-specific loop tuning?
**Affects**: [knowledge-loop]  
Sparse but highly structured graphs (ontologies, taxonomies) vs dense but shallow graphs (conversation logs) — same loop parameters or different?

### Q47: Can the loop be federated?
**Affects**: [knowledge-loop], [federation]  
Can usage patterns from Node A influence Node B's graph structure or embedding strategy? Or does each node run its own isolated loop?

## Hygiene & Consolidation (NEW - Feb 15, 2026)

### Q48: Automatic triple deduplication threshold?
**Affects**: [triples-atomic], [three-layer-architecture]  
Exact content hash match = definite duplicate. But what similarity threshold for fuzzy matches? 0.90 embedding similarity? 0.95? Configurable?

### Q49: Source independence criteria?
**Affects**: [three-layer-architecture], [epistemics-native]  
What counts as "independent corroboration"? Different session = independent? Different day? Different context? How do we prevent gaming?

### Q50: Confidence aggregation from sources?
**Affects**: [three-layer-architecture], [epistemics-native]  
If a triple has 5 sources with confidences [0.6, 0.7, 0.8, 0.6, 0.9], what's the aggregate? Mean? Weighted by source reliability? Median? Max?

### Q51: Summary scope determination?
**Affects**: [three-layer-architecture]  
How do we decide what triples to cluster into a summary? Per-entity (all facts about Chris)? Per-topic (all architecture preferences)? Per-session? Emergent clusters?

### Q52: Provenance depth in summaries?
**Affects**: [three-layer-architecture]  
When returning a summary, how much source detail to include? Just count ("5 sources")? Timestamps of observations? Full session links? Configurable by query?

## Dynamic Confidence & Topology

### Q53: Confidence computation caching?
**Affects**: [emergent-dimensions], [lazy-compute]  
Confidence dimensions are computed from topology at query time. Should we cache dimension scores for expensive queries, or always compute fresh? How long should cache live?

### Q54: Minimum graph structure for reliable confidence?
**Affects**: [emergent-dimensions], [knowledge-loop]  
How sparse can the graph be before topology-based confidence scores become unreliable? 100 triples? 500? Should we fall back to simpler heuristics for sparse graphs?

### Q55: Confidence score blending across dimensions?
**Affects**: [emergent-dimensions], [multi-dim-fusion]  
When multiple confidence dimensions are computed (source reliability, corroboration, freshness, etc.), how do we blend them into a single overall score? Fixed weights, context-dependent, learned from feedback?

### Q56: Contradicting triples detection algorithm?
**Affects**: [emergent-dimensions], [epistemics-native]  
How do we detect contradicting triples in the local neighborhood to compute internal consistency dimension? Semantic similarity of inverses? Explicit contradiction edges? LLM-based detection?

### Q57: Query-relevant subgraph boundary?
**Affects**: [emergent-dimensions], [graph-vector-duality]  
When computing domain applicability (centrality within query-relevant subgraph), how do we define the boundary? k-hops from query nodes? Similarity threshold? PageRank with query as seed?

### Q58: Should confidence dimensions be named at all?
**Affects**: [emergent-dimensions]  
If dimensions are purely topological, should we even label them (source reliability, corroboration, etc.) or treat them as unnamed axes? Does naming bias how they're used?

## PostgreSQL + Rust Sidecar Architecture (NEW - Feb 15, 2026)

### Q59: Sync protocol frequency?
**Affects**: [postgres-rust-architecture]  
Real-time sync (every insert), batched (every second), pull-on-query? Trade-off: consistency vs overhead.

### Q60: Embedding cache in Postgres?
**Affects**: [postgres-rust-architecture], [topology-embeddings]  
Should Rust cache computed topology embeddings in a Postgres table to speed startup? Or always recompute?

### Q61: Multiple Rust sidecars per Postgres?
**Affects**: [postgres-rust-architecture]  
Can we run multiple Rust sidecars against one Postgres instance for scale/redundancy? How do they coordinate?

### Q62: Memory budget for in-memory graph?
**Affects**: [postgres-rust-architecture], [bounded-memory]  
How much RAM can we assume? 1GB? 8GB? Should it be user-configurable with graceful degradation if exceeded?

## Budget-Bounded Operations (NEW - Feb 15, 2026)

### Q63: Default budget values per domain?
**Affects**: [budget-bounded-ops]  
Should different knowledge domains have different default budgets? Technical queries = more hops? Casual = fewer?

### Q64: Communicating partial results?
**Affects**: [budget-bounded-ops], [context-assembly]  
Do we tell users "Found 30 results in 50ms, stopped early"? Or silent? How do we indicate quality vs speed trade-off?

### Q65: Auto-tuning budgets from feedback?
**Affects**: [budget-bounded-ops], [inference-training-loop]  
Can we measure query quality (did user engage with results?) and auto-adjust budgets? Learn optimal budget per user/domain?

### Q66: Distributed budgets in federation?
**Affects**: [budget-bounded-ops], [federation]  
Multi-node queries need distributed budgets. How do we split budget across nodes? Fixed allocation vs dynamic?

## Self-Training Boundary (NEW - Feb 15, 2026)

### Q67: Minimum training data size?
**Affects**: [self-training-boundary]  
How many training pairs before fine-tuning is worthwhile? 1000? 5000? Domain-dependent?

### Q68: Which base model for boundary?
**Affects**: [self-training-boundary]  
Phi-3, Llama 3.2 1B, Qwen 2.5 1.5B, Mistral? Benchmark on decomposition task to decide?

### Q69: LoRA vs full fine-tuning?
**Affects**: [self-training-boundary]  
LoRA is faster and uses less memory. Full fine-tuning is more flexible. Which for boundary models?

### Q70: Sharing trained boundary models?
**Affects**: [self-training-boundary], [network-flows]  
Can we share trained models across users/engines? Privacy implications (models may encode training data).

### Q71: Separate models for decomposition vs recomposition?
**Affects**: [self-training-boundary]  
Should NL→triples and triples→NL use the same model or two specialized models? Task symmetry vs model efficiency.

## Engine + Network Product Vision (NEW - Feb 15, 2026)

### Q72: Engine-first or engine+network together?
**Affects**: [engine-network-product]  
Ship embeddable engine first, add network later? Or build both in parallel? Engine-first reduces scope, proves local value.

### Q73: Minimum viable federation protocol?
**Affects**: [engine-network-product], [federation]  
What's the smallest useful protocol? DID exchange + triple sync + corroboration? Or can we start even simpler?

### Q74: Spam/attack prevention on network?
**Affects**: [engine-network-product], [network-flows]  
How do we prevent spam, poisoning attacks, reputation gaming? Reputation + stake? Rate limiting? Proof-of-work?

### Q75: P2P infrastructure requirements?
**Affects**: [engine-network-product], [federation]  
Fully p2p (DHT for discovery, hole-punching for NAT)? Or some infrastructure (relays, discovery servers)? Tradeoff: purity vs practicality.

### Q76: Reference network or protocol only?
**Affects**: [engine-network-product]  
Should we operate a reference Valence network, or just publish the protocol and let networks emerge organically?

## Network Flows (NEW - Feb 15, 2026)

### Q77: Model sharing: weights, training data, or both?
**Affects**: [network-flows], [self-training-boundary]  
Sharing weights is convenient but large. Sharing training data is smaller but requires recipient to train. Both?

### Q78: Training data anonymization?
**Affects**: [network-flows]  
Should training pairs be anonymized before sharing (remove identifying entities)? Or rely on consent-gating alone?

### Q79: Conflicting domain models?
**Affects**: [network-flows]  
Two nodes share medical boundary models with different accuracies. Which does recipient use? Blend? Choose higher accuracy?

### Q80: Paid access to network resources?
**Affects**: [network-flows], [engine-network-product]  
Should the protocol support paid access (Engine A charges for high-quality model/compute)? Or reputation-only?

### Q81: Trustless compute delegation?
**Affects**: [network-flows]  
Can we use ZK proofs for verifiable computation (delegate embedding compute, verify correctness without trust)?

## Emergence & Loops (NEW - Feb 15, 2026)

### Q82: When to refactor vs build new?
**Affects**: [emergence-composition]  
How much technical debt is acceptable before refactoring? Balance between iteration speed and code quality.

### Q83: Loop maturity detection?
**Affects**: [self-closing-loops]  
Can we auto-detect when each loop is mature enough to transition phases? Metrics for "graph is dense enough", "model is accurate enough"?

### Q84: Loop transition: automatic or manual?
**Affects**: [self-closing-loops]  
Should phase transitions (LLM→local model, external→topology embeddings) be automatic or user-controlled?

### Q85: Federated loop behaviors?
**Affects**: [self-closing-loops], [network-flows]  
How do the three loops behave in federated settings? Does shared training data accelerate Loop 3? Does network structure improve Loop 1 embeddings?
