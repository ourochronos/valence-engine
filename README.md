# valence-engine

A context assembly engine with an epistemic knowledge substrate.

Not a database. Not a memory system. A system that constructs the best possible worldview for each inference call — so agents think better, not just remember more.

## What This Repo Is

This is the design space for valence-engine. We're using the structure of the ideas to organize the ideas — dogfooding the concept before writing code.

Every document has confidence scores. Open questions are tracked. Settled ideas are marked. The graph of concepts is the product.

## Core Thesis

> Stop treating the context window as a transcript. Treat it as an assembled worldview.

Instead of growing conversation history → compaction → loss, each inference call gets a freshly assembled context: the last few messages for thread continuity, plus exactly the knowledge that's relevant right now, surfaced by a multi-dimensional fusion query across an epistemic graph.

## Core Principles

1. **Triples as Atoms**: Knowledge decomposes to `(subject, relationship, object)` — not beliefs, not claims. Everything is triples: facts, taxonomy, provenance, confidence. The graph is self-describing.

2. **Three-Layer Architecture**: Triples (atomic facts) → Sources (provenance, never merged) → Summaries (LLM rendering at read boundary). Not three tables — TWO stored layers (triples + sources) plus on-demand rendering. Dedup at L1, preserve provenance at L2, render at L3. Summaries never hit storage.

3. **Topology-Derived Embeddings**: Vector embeddings generated from graph structure alone (spectral, random walks, message passing). No external model. No API dependency. Embeddings refine as graph grows.

4. **Graph-Vector Duality**: Graph = explicit structure (hard edges). Vector = implicit similarity (soft gradients). Two views of one system. Retrieval is hybrid: vectors find neighborhood, graph traversal finds precision.

5. **Complete Knowledge Loop**: LLM at boundary only (NL→triples on write, triples→NL on read). Engine is deterministic graph ops. Loop: LLM builds graph → topology generates embeddings → embeddings power retrieval → retrieval feeds LLM. Self-reinforcing.

6. **Cold/Warm Split**: Deterministic core (insert, link, retrieve, decay, evict, cluster) + inference enrichment (synthesis, labeling, curation). System works without inference at lower resolution.

7. **Inference as Training Loop**: Every query reshapes the substrate. No inner model, no weights. The LLM is purely at the boundary. Usage IS the training process.

8. **Value per Token**: Primary optimization metric. Maximize useful work per token consumed through precise context assembly, lazy compute, and tool mediation.

9. **Graceful Degradation**: Full function offline, without API, on slow hardware — just with lower resolution. Inference makes it richer but is never required.

10. **Bounded Memory**: Entire dataset is an LRU cache with hard boundary. Eviction IS forgetting. What matters gets retrieved, refreshed, and stays resident.

11. **Stigmergy + Progressive Summarization**: Structure emerges from use. Organization is continuous, proportional to value, and focused where it matters most.

12. **Dynamic Confidence**: Confidence dimensions are not stored metadata — they are computed from graph topology at query time. Source reliability, corroboration, temporal freshness, internal consistency, domain applicability — all emerge from structural analysis. Confidence is contextual: same triple can be high-confidence in one query context, low in another.

## Structure

```
docs/
  concepts/        — Core ideas, each as a node with confidence + status
  architecture/    — System design, layers, interfaces
  research/        — Competitive landscape, papers, prior art
  questions/       — Open questions that need resolution
  decisions/       — Settled decisions with reasoning chains
graph/
  NODES.md         — Index of all concept nodes
  EDGES.md         — Relationships between concepts
```

## Status

**Phase**: Design evolution — major breakthrough Feb 15 2026  
**Confidence**: Core architecture settled (triples, topology embeddings, knowledge loop, dynamic confidence), implementation details exploring  
**Origin**: Conversations between Chris Jacobs and Claude, Feb 2026  
**Latest**: Feb 15 2026 (v3) — Two-layer storage architecture (triples + sources, summaries are rendering not storage), dynamic confidence from topology (computed at query time, not stored metadata), complete self-reinforcing loop

## The Problem This Solves

**Current Valence (and most AI memory systems)** accumulate beliefs without consolidation:
- Query "Chris's preferences" → returns 15 overlapping similar beliefs
- No automatic deduplication (same insight from 5 sessions = 5 separate beliefs)
- Single-inference beliefs propagate as high-confidence truth (one conversation says "Chris dislikes X" → stored at 0.85 confidence forever)
- Corroboration dimension exists but never updates
- Retrieval returns mush (redundant, overlapping, unstructured)

**The three-layer architecture fixes this**:
- All observations decompose to ~4 unique triples (dedup at L1)
- Each triple has 3-5 independent sources (corroboration at L2)
- Retrieval returns 1-2 clean summaries (LLM rendering at L3, never stored)
- Confidence computed from graph topology at query time: source diversity, paths, recency, neighborhood structure

Two layers stored (triples + sources). One layer rendered (summaries). Clean. Consolidated. Honest.

## Lineage

Draws from:
- **Valence** — epistemic knowledge substrate (PostgreSQL, beliefs, confidence, federation) — and the hygiene problems we discovered at scale
- **pg_infinity** — Rust multi-dimensional search with adapter fusion
- **Bob/postgresql-singularity** — 5-dimensional memory (spatial, temporal, semantic, lexical, relational)
- **Stigmergy + progressive summarization** — structure emerges from use
- **Graph topology research** — spectral embeddings, DeepWalk/Node2Vec, graph neural networks without learned weights
