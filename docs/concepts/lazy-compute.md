# Lazy Compute

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [progressive-summarization], [stigmergy], [knowledge-lifecycle]

## Idea

Compute is spent proportional to demonstrated value, not ingestion volume. The system is lazy by design — it only processes data when use demands it.

## Rules

1. **Ingestion is cheap**: Store raw text + timestamp + source + trigram hash. No embeddings, no entity extraction, no classification.
2. **First retrieval triggers processing**: When a query touches L0 data, THEN compute embeddings, extract entities, create belief.
3. **Repeated use triggers refinement**: Graph connections, confidence updates, supersession — only on data that's proven relevant.
4. **Expensive operations are demand-driven**: BERT embedding, graph construction, tension detection — triggered by retrieval, not ingestion.

## Why This Works

The ratio of ingested data to useful data is typically 100:1 or higher. Processing everything upfront wastes 99% of compute. Lazy processing means you only pay for the 1% that matters — and you discover which 1% through natural use.

## Embedding Strategy

- L0: No embedding. Cheap fingerprint (trigram hash) for dedup only.
- L1: Full embedding on first retrieval. Quantized BERT via ort/candle. ~10ms on CPU.
- L2+: Embedding may be refreshed if the belief content is refined/superseded.

## Implication for System Resources

A valence-engine instance can ingest massive volumes (emails, messages, documents, code diffs) without compute pressure. The CPU/memory cost is determined by *query volume*, not ingestion volume. This is how it runs on normal systems.

## Open Questions

- Is trigram hash sufficient for L0 dedup, or do we need something smarter?
- How do we handle queries that touch large volumes of L0 data? (e.g., "find everything about X" when X appears in 500 raw ingestion events)
- Should there be a background "slow promote" for L0 data that's been sitting for a long time near frequently-accessed areas of the graph?
