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
- "Graph-based Agent Memory: Taxonomy, Techniques, and Applications" (arxiv 2602.05665, Feb 2026)
- "Memory in the Age of AI Agents" (arxiv 2512.13564, Jan 2026)
- Proper Epistemic Knowledge Bases (AAAI 2016) — nested multi-agent belief representation
- ICLR 2026 MemAgents Workshop — principled memory substrate for agents
