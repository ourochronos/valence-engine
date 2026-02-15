# Emergent Ontology

**Status**: settled  
**Confidence**: 0.85  
**Connections**: [stigmergy], [knowledge-graph], [progressive-summarization], [context-assembly]

## Idea

Entity types, relationship types, and structural patterns emerge from use rather than being predefined. No upfront schema. The ontology is discovered, not designed.

## How Entities Emerge

- A name appears repeatedly across ingestion events → entity candidate
- Co-occurrence patterns reveal relationships ("Chris" + "architecture" + "composable" → preference edge)
- The system doesn't need `Person(name="Chris")` declared — "Chris" crystallizes as an entity from density of reference
- Types are soft labels derived from relationship patterns, not hard categories

## How Relationships Emerge

- Co-occurrence in beliefs → implicit connection
- Explicit agent statements ("these two things are related") → explicit edge
- Tool results that link entities → inferred edge
- Contradiction between beliefs about the same entity → tension edge

## Focus

Queryable relationship surfacing over schema enforcement. The value is in answering "what's connected to X and how?" not in maintaining a clean taxonomy.

## Alignment with Stigmergy

Entities are ant trails. The ones that get walked (referenced, retrieved, connected) become paths. The paths that get walked become highways (high-confidence, well-connected nodes). Unused entities fade naturally.

## Open Questions

- Do we need any seed types at all, or is fully emergent viable?
- How do we handle entity resolution (same entity, different names)?
- At what density of reference does something crystallize from raw text into an entity?
- How do we prevent entity sprawl — thousands of low-value entity nodes?
