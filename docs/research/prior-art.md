# Prior Art (Our Own)

## Valence (current)
- PostgreSQL + pgvector knowledge substrate
- Beliefs with dimensional confidence, supersession chains
- Tensions, verification, reputation, federation protocol
- 56 MCP tools, OpenClaw plugin
- **What carries forward**: Epistemic model, belief lifecycle, federation concepts, trust/verification
- **What changes**: PostgreSQL → embedded engine, 56 tools → ~3-5, plugin → context assembly layer

## pg_infinity (Rust pgrx extension)
- Unified multi-dimensional search across backends
- Adapters: pgvector, Apache AGE (graph), FTS, ParadeDB, PostGIS, TimescaleDB
- JSON query → backend detection → adapter routing → result fusion
- **What carries forward**: Fusion concept, adapter pattern, multi-dimensional scoring
- **What changes**: Postgres extension → standalone engine, gluing backends → native indexes

## Bob / postgresql-singularity
- 5-dimensional memory: spatial, temporal, semantic, lexical, relational
- `bob.think()` and `bob.remember()` — stored procedures
- Consciousness system with autonomous thinking, pattern detection
- **What carries forward**: Multi-dimensional search, the 5 dimensions, autonomous refinement
- **What changes**: Postgres stored procedures → Rust native, separate dimensions → unified fusion

## Ourochronos
- Self-improving development system ("building Bob by being proto-Bob")
- pg_infinity as the extension component
- Thought capture tools, tangent parking
- **What carries forward**: The meta-pattern — using the thing to build the thing

## Key Lessons from Prior Work

1. **Postgres is too heavy** for zero-dependency deployment
2. **Separate backend fusion is lossy** — need unified scoring
3. **56 tools overwhelm** — the LLM and the token budget
4. **Auto-capture produces junk** without intent understanding
5. **The hygiene function works** — organize through use, not batch jobs
6. **5 dimensions are real** but need native support, not SQL glue
