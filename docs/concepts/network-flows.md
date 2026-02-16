# What Flows Through the Network

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [engine-network-product], [federation], [self-training-boundary], [knowledge-loop]

## Core Principle

**Everything that decomposes to triples can flow through the network.**

The network doesn't need different protocols for different content types. It's all triples, all the same trust/consent framework, all the same verification mechanism.

The protocol is **content-agnostic**. It cares about structure (triples), provenance (where did this come from?), and consent (who can see this?). The semantic meaning is in the graph, not the protocol.

## What Flows

### 1. Beliefs (Knowledge Triples)
The obvious case:
- `(Chris, prefers, composable_architectures)`
- `(Rust, enables, memory_safety_without_GC)`
- `(valence-engine, uses, PostgreSQL_for_persistence)`

**Why share**: Corroboration (multiple independent sources → higher confidence), discovery (learn from others' knowledge).

**Consent**: User controls which beliefs share (per-belief or per-domain consent).

### 2. Skills (Capability Triples)
Skills decompose to triples:
- `(write_file_skill, requires, file_path_parameter)`
- `(write_file_skill, produces, file_created_event)`
- `(web_search_skill, has_cost, $0.001_per_call)`

**Why share**: Agent learns new capabilities from other agents, understands what peers can do.

**Consent**: Skills are often public (capability advertisement), but can be private (proprietary tools).

### 3. Trust Scores (Epistemic Triples)
Trust relationships are triples:
- `(Node_A, trusts, Node_B, confidence=0.85)`
- `(Node_B, verified_claim_from, Node_C, timestamp=...)`
- `(Node_A, distrust, Node_D, reason=spam)`

**Why share**: Trust propagates (friend-of-friend), reputation builds, bad actors are isolated.

**Consent**: Trust scores are often shared (public reputation), but can be private (personal blocklists).

### 4. Reputation (Network Metadata)
Reputation metadata as triples:
- `(Node_A, shared, 147_beliefs)`
- `(Node_A, corroboration_rate, 0.73)`  ← 73% of shared beliefs were corroborated
- `(Node_A, contradiction_rate, 0.05)`  ← 5% contradicted by other sources

**Why share**: Enables informed trust decisions (should I accept beliefs from this node?).

**Consent**: Reputation is public by design (opt-in to network = opt-in to reputation tracking).

### 5. Models (Boundary Model Triples)
Trained boundary models decompose to triples:
- `(boundary_model_v3, trained_on, 5000_decomposition_pairs)`
- `(boundary_model_v3, accuracy, 0.94)`
- `(boundary_model_v3, domain, software_architecture)`
- `(boundary_model_v3, base_model, Phi-3-mini)`

**Why share**: One engine's training data improves another's model. Specialized domains benefit (medical, legal, technical).

**Consent**: Models can be public (open weights) or private (proprietary). Model metadata (performance, domain) often shared even if weights aren't.

### 6. Training Data (Self-Training Loop Closure)
Training pairs for boundary models:
- `(training_pair_1047, input, "Chris prefers composable architectures")`
- `(training_pair_1047, output, [(Chris, prefers, composable_architectures)])`
- `(training_pair_1047, validated, true)`  ← User confirmed correctness

**Why share**: Close the self-training loop across the network. Engine A's usage improves Engine B's boundary model.

**Consent**: Training data is sensitive (contains user knowledge). Shared only with explicit consent, often anonymized.

### 7. Compute (Delegated Operations)
Compute requests as triples:
- `(Node_A, requests, topology_embedding_computation, graph_size=50000)`
- `(Node_B, offers, embedding_computation_service, cost=$0.01_per_1000_nodes)`
- `(Node_A, delegates, embedding_computation_to, Node_B)`

**Why share**: Heavy compute (topology embeddings on 100K node graph) can be delegated to trusted peers with more resources.

**Consent**: Delegation requires trust (you're sharing graph structure). Only to high-trust nodes.

## What Does NOT Flow

### Raw L0 Data
- Emails, documents, conversation transcripts, tool outputs
- **Why not**: Privacy. L0 is personal, sensitive, high-entropy.
- **Alternative**: Share the triples extracted from L0, not L0 itself.

### Graph Structure
- The full adjacency list, neighborhood topology, clustering
- **Why not**: Structure reveals sensitive patterns (who connects to what).
- **Alternative**: Share individual triples, let receiving node rebuild local structure.

### Working Set / Session State
- Active conversation context, current inference state
- **Why not**: Ephemeral, not knowledge. No value in sharing.
- **Alternative**: Don't share. This is local engine state only.

### Embeddings
- Vector representations of nodes
- **Why not**: Embeddings are model-dependent. Sharing embeddings assumes same embedding model.
- **Alternative**: Share triples, let receiving node compute its own embeddings from topology.

## The Self-Training Loop Across the Network

This is where it gets powerful:

### Local Loop (Single Engine)
1. LLM decomposes NL → triples (generates training data)
2. Store training pairs
3. Fine-tune local boundary model
4. Replace LLM with local model (scaffolding falls away)

### Network Loop (Federated Engines)
1. **Engine A**: User interacts, generates 1000 training pairs (domain: medical)
2. **Engine A**: Fine-tunes boundary model on medical domain
3. **Engine A**: Shares model metadata: `(boundary_model_medical_v1, accuracy, 0.96, domain=medical)`
4. **Engine B**: Sees shared model, requests access (consent required)
5. **Engine A**: Shares model weights or training data (depending on consent)
6. **Engine B**: Uses Engine A's model or training data to improve own medical domain performance
7. **Engine B**: Contributes back (shares improvements, new training data, or refined model)

**Result**: Collective intelligence. Engines specialize in domains, share expertise, improve together.

### Distributed Training
- Engine A: Strong in software architecture (10K training pairs)
- Engine B: Strong in medical terminology (8K training pairs)
- Engine C: Strong in legal analysis (6K training pairs)

Each engine shares its specialized boundary model. Other engines can:
- **Use directly**: Load specialized model for domain-specific queries
- **Blend**: Combine multiple domain models (architecture + medical for healthtech)
- **Fine-tune further**: Start from shared model, add own domain data

**Network effect**: 1000 engines training in 1000 domains = 1000 specialized models available to all (with consent/trust gating).

## Capability Sharing (Not Just Information)

Traditional knowledge sharing:
- **What**: Facts, documents, beliefs
- **Result**: Recipient knows more things

Valence network sharing:
- **What**: Triples, models, skills, compute
- **Result**: Recipient can DO more things

### Example: Skill Sharing
- **Engine A** has access to a proprietary research database (skill: `query_research_db`)
- **Engine B** doesn't have access, but trusts Engine A
- **Engine B** asks: "What research exists on topic X?"
- **Engine A** runs `query_research_db(topic=X)`, extracts triples, shares results as beliefs
- **Engine B** gains knowledge it couldn't have acquired alone

**The network shares capability**, not just information.

### Example: Compute Delegation
- **Engine A** (mobile device) has 100K node graph, needs topology embeddings
- **Engine A** trusts **Engine B** (home server with GPU)
- **Engine A** sends graph structure to **Engine B** (encrypted, consent-gated)
- **Engine B** computes embeddings, returns results
- **Engine A** uses embeddings locally

**The network shares compute**, not just storage.

## Content-Agnostic Protocol

The network protocol doesn't care what the triples mean. It only enforces:

### Structure
- All content decomposes to triples: `(subject, predicate, object)`
- Triples have provenance: `source_node_did`, `timestamp`, `confidence`
- Triples have consent metadata: `sharing_policy`, `trust_requirements`

### Trust
- Every triple has a trust chain (who vouches for this?)
- Corroboration events increase trust (multiple sources agree)
- Contradiction events decrease trust (sources conflict)

### Consent
- Every triple has access control (who can see this?)
- Sharing requires explicit consent (no implicit broadcast)
- Revocation is supported (remove trust signal, not delete data)

**Same protocol for beliefs, skills, training data, models, reputation, compute requests.** All triples, all trust-gated, all consent-controlled.

## Open Questions

- Should models be shared as weights, training data, or both? (Trade-off: size vs privacy)
- How do we prevent poisoning attacks? (malicious training data degrades models)
- Can compute delegation be trustless? (ZK proofs of correct computation?)
- Should training data be anonymized before sharing? (remove identifying triples?)
- How do we handle conflicting domain models? (two medical models with different accuracies)
- Should the network support paid access? (Engine A charges for high-quality model access)

## Related Concepts

- [engine-network-product]: Why sharing capability matters (engine + network vision)
- [self-training-boundary]: The local training loop that closes across the network
- [federation]: The technical protocol for how triples flow
- [knowledge-loop]: The self-reinforcing loop extends to network scale
