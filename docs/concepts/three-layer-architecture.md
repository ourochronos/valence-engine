# Three-Layer Architecture

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [triples-atomic], [progressive-summarization], [knowledge-lifecycle], [epistemics-native]

## Idea

The knowledge graph has three conceptual layers, but only TWO are stored:

1. **Layer 1: Triples** — Atomic facts. `(subject, relationship, object)` — **STORED**
2. **Layer 2: Sources** — Provenance records. Which triples came from where, with FK links — **STORED**
3. **Layer 3: Summaries** — Recomposed clusters of triples for human/LLM consumption — **RENDERED, NOT STORED**

This isn't three tables. It's two stored layers (graph + provenance) plus a rendering layer (LLM at read boundary).

## Layer 1: Triples (The Atoms)

Every piece of knowledge decomposes to this level:
```
(Chris, prefers, composable_architectures)
(composable_architectures, is_a, design_philosophy)
(design_philosophy, is_a, concept)
```

All deduplication happens here. If 5 different sessions observe "Chris prefers composable architectures", that's ONE triple with 5 sources, not 5 beliefs.

Properties:
- Immutable once created
- Content-addressed (hash of subject+relationship+object)
- No timestamps at this layer (triples are timeless facts)
- No confidence scores here (confidence comes from sources)

## Layer 2: Sources (The Provenance)

Each source is a record of where a triple came from:
```
source_id: s_47
triple_id: t_123 (Chris, prefers, composable_architectures)
origin: session_12
observed_at: 2026-02-15T22:00:00Z
observer: agent_claude
context: "discussing valence-engine design"
confidence: 0.75 (initial)
```

Same triple can have many sources. This is how corroboration works:
- Triple t_123 has 5 sources → high corroboration → confidence boost
- Triple t_456 has 1 source → low corroboration → confidence penalty

Sources are NEVER merged. Each remains independent so provenance is preserved.

Properties:
- One source per observation
- Temporal (has timestamps)
- Has confidence dimensions
- Links to session/conversation/tool that produced it
- Immutable (new observation = new source, not update)

## Layer 3: Summaries (The Recompositions)

**CRITICAL: Summaries are NOT stored. They are a rendering produced by inference at the read boundary.**

When you query the system:
1. Engine returns clusters of triples (based on cosine similarity to query)
2. LLM at the boundary recomposes those triples into natural language
3. Summary is generated fresh each time, never hits storage

Example rendering:
```
Query: "What are Chris's architecture preferences?"

Triples returned:
  (Chris, prefers, composable_architectures)
  (Chris, dislikes, framework_lock_in)
  (composable_architectures, is_a, design_philosophy)

LLM renders as:
  "Chris strongly prefers composable architectures over 
   monolithic designs (observed across 5 sessions, 
   high confidence: 0.92)"
```

Summaries are **disposable by nature** — they don't exist until someone asks, and they vanish after delivery.

Properties:
- Generated on-demand at query time (never stored)
- Rendered by LLM from triple clusters
- Can be tuned for audience (technical vs casual)
- Can be scoped by query context
- No storage cost, no staleness problem
- Always reflect current graph state

## How This Fixes the Hygiene Problem

**Current Valence problem**: Beliefs accumulate without consolidation. Query for "Chris preferences" returns 15 overlapping similar beliefs.

**With three layers**:
1. All 15 observations decompose to ~4 unique triples (dedup at L1)
2. Each triple has 3-5 sources showing independent observations (provenance at L2)
3. Retrieval returns 1-2 summaries that compose the triples into coherent statements (presentation at L3)

Clean, consolidated, traceable.

## Relationship to Progressive Summarization

This IS progressive summarization, but made structural:
- L0 (raw ingestion) → cheap storage, not yet processed
- L1 (triples) → first extraction pass, atomic facts
- L2 (sources) → provenance and confidence tracking
- L3 (summaries) → composed understanding
- L4 (insights) → synthesized patterns across summaries

The pyramid is one graph at different resolutions.

## Benefits

1. **Deduplication is automatic**: Happens at triple layer via content addressing
2. **Provenance is preserved**: Sources never merge, full chain always available
3. **Retrieval is clean**: Return summaries, user drills into triples if needed
4. **Confidence is honest**: Aggregated from source diversity, not single observation
5. **System is regenerable**: Lose all summaries? Rebuild from triples. Lose all triples? Can't rebuild (triples are ground truth).

## Storage Implications

- Triples table: ~10-100x more rows than current beliefs (one belief → many triples)
- Sources table: ~1-5x current beliefs (one observation per source)
- Summaries: **zero storage** (rendered on-demand, never stored)

Net: more rows in triples/sources, but no summary storage overhead. Clean separation of data (stored) from presentation (rendered).

## Query Pattern

```
User: "What do I know about Chris's preferences?"

1. Vector search on summaries for "Chris preferences"
   → finds sum_88, sum_91, sum_103

2. Return summaries with confidence + source count
   → "Chris strongly prefers composable architectures (5 sources, conf 0.92)"
   → "Chris dislikes framework lock-in (3 sources, conf 0.81)"

3. User drills in: "Show me the sources for the first one"
   → Walk from sum_88 → t_123, t_124, ... → s_47, s_51, s_62, ...
   → Return: session IDs, timestamps, contexts

4. User asks: "When did I first observe this?"
   → Sort sources by observed_at → return earliest
```

Summaries for speed. Triples for precision. Sources for provenance.

## Open Questions

- How do we decide summary scope at query time? (per-entity, per-topic, per-session?)
- Can the LLM see raw triples directly, or always through summaries?
- Should there be a summary cache for expensive queries, or always render fresh?
- How do we tune summary verbosity based on query context? (terse vs detailed)
- Can users request "show me the triples" instead of the rendered summary?
