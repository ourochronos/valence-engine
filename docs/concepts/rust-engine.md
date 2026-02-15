# Rust Engine

**Status**: exploring  
**Confidence**: 0.75  
**Connections**: [multi-dim-fusion], [lazy-compute], [epistemics-native], [embedded-deployment]

## Idea

A purpose-built Rust binary that is to knowledge what SQLite is to relational data. Single embeddable binary. No external dependencies. The context assembly engine.

## Architecture Layers

```
┌─────────────────────────────────────┐
│  MCP / HTTP / stdio interface       │  Protocol layer
├─────────────────────────────────────┤
│  Context assembly + fusion query    │  The core product
├────────┬────────┬─────────┬─────────┤
│ Vector │ Graph  │ Belief  │Temporal │  Unified index layer
│ Index  │ Index  │ Index   │ Index   │
├────────┴────────┴─────────┴─────────┤
│  Embedded BERT (ort/candle)         │  No external API
├─────────────────────────────────────┤
│  Storage engine (RocksDB/sled)      │  Persistent, crash-safe
├─────────────────────────────────────┤
│  Federation protocol                │  P2P sync, DIDs
└─────────────────────────────────────┘
```

## Why Rust

- Single binary distribution — `npm install` or `pip install` with native addon
- Performance — <50ms context assembly on normal hardware
- Memory safety without GC — predictable latency
- Embeddable — can run as library in Node.js (napi-rs) or Python (PyO3)
- The ecosystem: ort (ONNX Runtime), sled/RocksDB, petgraph

## Embedded Embeddings

- Quantized BERT (all-MiniLM-L6-v2): 22M params, ~80MB, ~10ms per embedding on CPU
- Via `ort` (ONNX Runtime Rust bindings) or `candle` (HuggingFace Rust ML)
- No external API needed — privacy by default, works offline
- Model ships with binary or downloads on first use (like QMD)

## Storage Options

- **RocksDB**: Battle-tested, good for write-heavy workloads, used by CockroachDB/TiKV
- **sled**: Pure Rust, embedded, lock-free — but less mature
- **SQLite**: Maximum compatibility, but less control over index structures
- **Custom**: Purpose-built for the specific access patterns we need

## Open Questions

- Build time vs payoff: months of Rust work. Is there a simpler path that proves the concept first?
- Graph storage: purpose-built adjacency structures, or lean on an existing Rust graph library (petgraph)?
- Model distribution: ship 80MB of BERT weights in the binary, or download on first use?
- How do we do the Python bridge for the existing Valence codebase during transition?
- Can we prototype the context assembly logic in Python/TypeScript first, then port to Rust?
