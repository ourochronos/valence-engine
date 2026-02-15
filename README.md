# valence-engine

A context assembly engine with an epistemic knowledge substrate.

Not a database. Not a memory system. A system that constructs the best possible worldview for each inference call — so agents think better, not just remember more.

## What This Repo Is

This is the design space for valence-engine. We're using the structure of the ideas to organize the ideas — dogfooding the concept before writing code.

Every document has confidence scores. Open questions are tracked. Settled ideas are marked. The graph of concepts is the product.

## Core Thesis

> Stop treating the context window as a transcript. Treat it as an assembled worldview.

Instead of growing conversation history → compaction → loss, each inference call gets a freshly assembled context: the last few messages for thread continuity, plus exactly the knowledge that's relevant right now, surfaced by a multi-dimensional fusion query across an epistemic graph.

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

**Phase**: Design exploration  
**Confidence**: Early — many open questions, core thesis is high-confidence  
**Origin**: Conversations between Chris Jacobs and Claude, Feb 15 2026

## Lineage

Draws from:
- **Valence** — epistemic knowledge substrate (PostgreSQL, beliefs, confidence, federation)
- **pg_infinity** — Rust multi-dimensional search with adapter fusion
- **Bob/postgresql-singularity** — 5-dimensional memory (spatial, temporal, semantic, lexical, relational)
- **Stigmergy + progressive summarization** — structure emerges from use
