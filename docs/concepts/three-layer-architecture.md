# Three-Layer Architecture

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [triples-atomic], [progressive-summarization], [knowledge-lifecycle], [epistemics-native]

## Idea

The knowledge graph has three scales, not three separate systems:

1. **Layer 1: Triples** — Atomic facts. `(subject, relationship, object)`
2. **Layer 2: Sources** — Provenance records. Which triples came from where, with FK links
3. **Layer 3: Summaries** — Recomposed clusters of triples for human/LLM consumption

This isn't three tables. It's three views of the same graph.

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

Human-readable or LLM-consumable recompositions of triple clusters:
```
summary_id: sum_88
content: "Chris strongly prefers composable architectures 
          over monolithic designs (observed across 5 sessions, 
          high confidence)"
constituent_triples: [t_123, t_124, t_125, t_126, t_127]
generated_at: 2026-02-15T22:30:00Z
confidence: 0.92 (aggregated from sources)
```

Summaries are **disposable and regenerable**. You can delete all summaries and regenerate them from triples + sources at any time.

Properties:
- Generated on-demand or cached
- Can be tuned for audience (technical vs casual)
- Can be scoped (summary of Chris's preferences vs summary of all architecture beliefs)
- Linked back to constituent triples
- This is what retrieval returns (not raw triples)

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
- Summaries table: ~0.1-0.5x current beliefs (summaries consolidate)

Net: more total rows, but better organized, and most retrieval queries only touch summaries.

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

- Should summaries be auto-generated on every triple cluster, or only on-demand?
- How do we decide summary scope? (per-entity, per-topic, per-session?)
- Can the LLM see raw triples directly, or always through summaries?
- What's the regeneration cost if we delete all summaries? (acceptable for maintenance?)
- How do we handle summary versioning when underlying triples change?
