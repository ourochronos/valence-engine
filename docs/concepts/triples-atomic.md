# Triples as the Atomic Unit

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [three-layer-architecture], [emergent-ontology], [epistemics-native], [knowledge-lifecycle]

## Idea

The atomic unit of knowledge is the triple: **subject → relationship → object**. Not the belief. Not the claim. The triple.

Everything decomposes into triples:
- Facts: `(pgvector, is_a, database)`
- Taxonomy: `(database, is_a, infrastructure)`
- Provenance: `(belief_47, sourced_from, session_12)`
- Confidence: `(belief_47, confidence_dimension, source_reliability)`
- Temporal: `(Chris, preferred, composability, valid_from: 2024-01-15)`
- Spatial: `(meeting, occurred_at, SF_office)`

Current Valence "beliefs" are triples trying to get out. Domain paths, confidence dimensions, supersession chains — all of these are graph relationships stored in ad-hoc column formats.

## Self-Describing System

The ontology describes itself using itself:
- `(is_a, is_a, relationship_type)`
- `(relationship_type, is_a, concept)`
- `(confidence_dimension, is_a, attribute_type)`

No special cases. No meta-schema separate from the schema. The graph is one continuous structure.

## What This Means for Storage

Instead of:
```sql
beliefs(
  id, content, domain_path, confidence_source, 
  confidence_method, valid_from, created_by
)
```

We have:
```
triples(subject_id, relationship_id, object_id, metadata)
```

Where a "belief" is just a cluster of triples with a shared origin:
```
(belief_47, has_content, "Chris prefers composable architectures")
(belief_47, has_confidence_score, 0.85)
(belief_47, sourced_from, session_12)
(belief_47, observed_by, agent_claude)
(belief_47, created_at, 2026-02-15T22:00:00Z)
(Chris, prefers, composable_architectures)
(composable_architectures, is_a, design_philosophy)
```

## Relationship to Three-Layer Architecture

- **Layer 1 (Triples)**: The atomic facts
- **Layer 2 (Sources)**: Provenance triples linking back to where facts came from
- **Layer 3 (Summaries)**: Composed clusters of triples, regenerable

The whole system is one graph at different scales.

## Benefits

1. **Deduplication happens naturally**: Same triple from different sources = corroboration, not duplication
2. **Ontology emerges from use**: No upfront entity types or relationship schemas
3. **Queries are graph traversals**: "What does Chris prefer?" = walk from Chris node via prefers edges
4. **Federation is clean**: Share atomic triples, not complex belief objects
5. **Provenance is just more triples**: Every fact links back to its sources via the same mechanism

## The Current Problem (Valence Hygiene)

Current Valence stores beliefs as opaque text blobs with metadata columns. This causes:
- Duplicate beliefs that should be the same triple with multiple sources
- Retrieval returns overlapping similar results (belief A, belief B, belief C all saying "Chris likes X")
- No natural deduplication layer
- Confidence can't properly aggregate across sources

Decomposing to triples fixes all of this. Dedup happens at the triple layer. Provenance stays separate at the source layer. Retrieval returns summaries that link to triples.

## Open Questions

- How granular should triples be? (Single assertion per triple, or can objects be complex?)
- How do we handle temporal validity at the triple level vs belief level?
- What's the migration path from current belief-based Valence to triple-based?
- How do we serialize triples for LLM consumption without overwhelming context?
