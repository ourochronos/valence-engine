# Competitive Landscape (Feb 2026)

## Direct Competitors

### Mem0 (YC-backed, most commercially mature)
- Cloud or self-hosted (Docker + Postgres/Qdrant + OpenAI)
- Graph memory (nodes + relationships), versioned APIs
- Official OpenClaw plugin: `openclaw plugins install @mem0/openclaw-mem0`
- 5 tools, 30-second setup
- **Strength**: Fastest path to production, great SDKs
- **Weakness**: Requires external embedding API even self-hosted. No epistemics. Cloud-first.

### Zep (temporal knowledge graph)
- Cloud-first, community edition for self-hosted
- Knowledge graph + Graph RAG, 200ms retrieval
- **Strength**: Best benchmark scores on DMR, strong graph approach
- **Weakness**: Best features cloud-only. 600K+ token overhead for graph.

### Letta/MemGPT (the OG)
- Self-editing memory, OS-inspired architecture
- Showed filesystem beats specialized tools at 74% on LoCoMo
- **Strength**: Principled architecture, Agent File format, model-agnostic
- **Weakness**: Complex, more agent framework than memory layer

### OpenClaw memory-core (what we replace)
- Markdown files + SQLite vector search + optional QMD sidecar
- **Strength**: Zero dependencies, battle-tested, 190K users
- **Weakness**: No structure, no confidence, no provenance

## ClawHub Memory Ecosystem
- memory-tools: confidence scoring, decay, semantic search
- ontology: typed knowledge graph with constraints
- memory-manager: compression detection, auto-snapshots
- compound-engineering: nightly consolidation loops
- openclaw-mem0-v2: identity mapping, graph memory, sleep mode

## What Nobody Has
1. Epistemics-native engine (confidence as query dimension)
2. Context assembly (replace history with assembled worldview)
3. Federation with nested epistemic states
4. Lazy compute (process on retrieval, not ingestion)
5. Tool mediation with intent capture
6. Emergent ontology through use
7. Zero-dependency embedded deployment with built-in embeddings

## Key Academic Work

### Graph-based Agent Memory (arxiv 2602.05665, Feb 2026)
Survey paper identifying the frontier: graph memory transitioning from passive log to structured topological model of experience.

**Key structures identified**:
- Knowledge graph (facts/relationships)
- Hierarchical (nested contexts)
- Temporal graph (time-aware)
- Hypergraph (n-ary relationships)
- Hybrid architectures (combining multiple)

**Memory lifecycle**: extraction → storage → retrieval → evolution

**Evolution mechanisms**:
- Consolidation (merging similar memories)
- Graph reasoning (inference over structure)
- Reorganization (structure updates based on use)

**Critical insight: Knowledge-Experience Decoupling**  
Separating factual knowledge from experiential memory in hybrid graph architectures. This validates Valence's belief/session separation and adds the temporal graph and hypergraph dimensions we're missing.

### Memory in the Age of AI Agents (arxiv 2512.13564, Jan 2026)
Comprehensive survey of agent memory systems. Benchmark results show agents with memory use 12× fewer tokens and complete requests 3× faster than agents without memory.

### Proper Epistemic Knowledge Bases (AAAI 2016)
Formal foundation for nested multi-agent belief representation: "Agent A believes that Agent B knows X." This is the theoretical basis for federation with nested epistemic states.

### ICLR 2026 MemAgents Workshop
Workshop focused on principled memory substrates for agents. Key theme: moving from ad-hoc memory solutions to formally grounded architectures.
