# Engine + Network: The Complete Product Vision

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [federation], [rust-engine], [privacy-sovereignty], [epistemics-native]

## The Product Decomposition

**valence-engine** and **Valence** are NOT the same thing. They're two layers of the same vision:

### valence-engine: The Brain (This Repo)
The portable, embeddable knowledge substrate engine. Runs anywhere:
- **Sidecar**: Standalone process alongside an app
- **Embedded**: Library in Node.js (napi-rs), Python (PyO3), or native apps
- **WASM**: Browser-based, client-side knowledge engine
- **Edge**: Mobile devices, IoT, air-gapped environments
- **Inside models**: Future LLMs embed the engine for structured memory

**Every agent, model, and person gets their own engine instance.**

Each instance has:
- Its own knowledge graph (triples, sources, provenance)
- Its own topology embeddings (computed from local graph)
- Its own dynamic confidence (context-dependent, query-time)
- Its own memory boundary (LRU eviction, bounded by resources)

The engine is **sovereign** — no shared state, no central authority, full user control.

### Valence: The Nervous System (The Network)
The protocol and infrastructure that connects engine instances. Enables:
- **Federation**: Engines discover and connect to each other
- **DIDs**: Decentralized identity (every engine has a DID, every user/agent has a DID)
- **Trust propagation**: Reputation flows through the network based on corroboration quality
- **Consent-gated sharing**: Knowledge shares only with explicit user consent
- **Verification**: Cryptographic proof of provenance, tamper-evident logs
- **Reputation**: Nodes build trust through accurate sharing, lose trust through bad data

**Valence is the protocol for how knowledge moves between minds (human and artificial) with trust, provenance, consent, and auditability.**

## The Brain/Nervous System Analogy

### Brains (Engines)
- **Independent**: Each brain processes its own knowledge
- **Sovereign**: Full control over what's stored, retrieved, shared
- **Specialized**: Can tune to specific domains, use cases, memory budgets
- **Offline-capable**: Work fully without network connectivity
- **Portable**: Run on your hardware, your terms

### Nervous System (Network)
- **Connectivity**: Enables brains to communicate
- **Selective**: Not all brains connect to all brains (trust-gated)
- **Bidirectional**: Knowledge flows both ways (when permitted)
- **Filtered**: Consent and trust control what flows
- **Emergent**: Collective intelligence emerges without central coordination

## Why This Split Matters

### Without the engine: No brains, just a nervous system
Federation without local substrate = sharing beliefs with no structure. Existing federated systems (Mastodon, Matrix, AT Protocol) share content but don't build knowledge graphs, compute confidence, or enable epistemics-native operations.

**Problem**: Sharing unstructured beliefs doesn't create understanding. It creates noise.

### Without the network: Islands of knowledge
Local engines with no federation = every agent rebuilds the world from scratch. No shared learning, no corroboration across sources, no trust building through verification.

**Problem**: Isolated engines can't leverage collective intelligence. Every instance is limited to what its user knows.

### Together: Distributed cognition
- Local engines build rich knowledge graphs (triples, confidence, provenance)
- Network enables selective sharing (beliefs, skills, trust scores, reputation)
- Corroboration happens across instances (multiple engines observe same pattern → confidence increases)
- Trust propagates (accurate sharers gain reputation, inaccurate sharers lose it)
- Collective intelligence emerges (1000 engines sharing via trust protocol > 1000 isolated engines)

## Who Uses What?

### Individual Users
- **Engine**: Personal knowledge substrate (runs on laptop, phone, home server)
- **Network**: Connect to friends, trusted sources, domain experts
- **Value**: Private memory + selective sharing with people you trust

### AI Agents
- **Engine**: Structured memory (better than conversation history)
- **Network**: Agent-to-agent knowledge exchange mediated by trust
- **Value**: Agents learn from each other without central training

### Organizations
- **Engine**: Team knowledge substrate (shared instance or federated personal instances)
- **Network**: Connect to partners, vendors, industry peers
- **Value**: Organizational memory + trust-gated external knowledge

### Researchers
- **Engine**: Research notes, citations, corroboration tracking
- **Network**: Peer review, cross-lab verification, reproducibility
- **Value**: Epistemic rigor + transparent provenance

## What Makes This Different from Existing Systems?

### vs. Vector Databases (Pinecone, Weaviate, etc.)
- **Them**: Store embeddings, no graph structure, no provenance, no confidence
- **Us**: Graph + vectors (duality), provenance chains, dynamic confidence

### vs. Knowledge Graphs (Neo4j, Stardog, etc.)
- **Them**: Explicit relationships only, no vector search, no federation
- **Us**: Graph + vectors, topology embeddings, federated trust protocol

### vs. Federated Social (Mastodon, BlueSky, etc.)
- **Them**: Share posts/content, no knowledge graphs, no confidence tracking
- **Us**: Share triples/beliefs, epistemic structure, confidence/provenance/corroboration

### vs. Blockchain Knowledge Systems (Ceramic, OrbitDB, etc.)
- **Them**: Immutable logs, public by default, crypto-first
- **Us**: Mutable with supersession chains, private by default, crypto where needed (DIDs, verification)

### vs. Personal Knowledge Management (Obsidian, Roam, etc.)
- **Them**: User-structured notes, manual linking, no inference
- **Us**: Auto-structured triples, emergent ontology, inference at boundary only

**We're the first system that combines**: Graph structure + vector search + dynamic confidence + federated trust + embeddable deployment.

## The Path to Network Effects

### Phase 1: Single-Player Engine (Now)
- Build the engine (Postgres + Rust, triples, topology embeddings, context assembly)
- Prove the local value (better knowledge substrate for AI agents)
- No network yet — every engine is isolated

**Goal**: Nail the local experience. Engine must be valuable standalone.

### Phase 2: Pairwise Sharing (Next)
- Two engines connect (manual DID exchange)
- Share selected beliefs (consent-gated)
- Corroboration increases confidence (both engines benefit)
- Trust scoring begins (did shared beliefs prove accurate?)

**Goal**: Prove federation value. Show that sharing improves both participants.

### Phase 3: Small Networks (Trust Circles)
- 10-50 engines in a trust circle (friends, team, community)
- Trust propagates (friend-of-friend with decay)
- Reputation emerges (consistent accurate sharers gain influence)
- Collective knowledge grows (patterns observed across circle)

**Goal**: Reach critical mass. Network effects become obvious.

### Phase 4: Open Network (The Vision)
- Thousands of engines (personal, agent, org instances)
- Public discovery (DIDs, capability advertisements)
- Reputation markets (pay for high-reputation knowledge access)
- Emergent intelligence (patterns no single engine could see)

**Goal**: Distributed cognition at scale. The network becomes smarter than any node.

## Open Questions

- Should the engine and network launch together, or engine-first? (Leaning engine-first)
- What's the minimum viable federation protocol? (DID exchange + triple sync + corroboration)
- How do we prevent spam/attacks on the network? (reputation + stake + rate limiting)
- Can the network be fully p2p, or do we need some infrastructure? (DHT for discovery, relays for NAT traversal)
- How do we handle network partitions? (offline-first, eventual consistency when reconnected)
- Should there be a reference Valence network, or just the protocol? (Protocol only, let networks emerge)

## Related Concepts

- [federation]: The network layer technical details
- [rust-engine]: The engine implementation
- [privacy-sovereignty]: User control over data (engine + network must preserve this)
- [epistemics-native]: Both engine and network are epistemics-native (confidence, provenance, trust)
