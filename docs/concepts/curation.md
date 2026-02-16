# Curation

**Status**: exploring  
**Confidence**: 0.75  
**Connections**: [deterministic-core], [tool-mediation], [epistemics-native], [bounded-memory]

## Idea

The LLM gets explicit curation tools to actively shape the knowledge substrate: correct, enrich, link, pin, surface. This allows deliberate correction alongside the deterministic engine's organic mechanisms.

## Curation Tools

- **Correct**: Fix a belief's content or confidence. Overrides organic signals with explicit judgment.
- **Enrich**: Add context, implications, or metadata to an existing node.
- **Link**: Explicitly create connections that co-retrieval hasn't surfaced yet.
- **Pin**: Protect a node from decay and eviction. For knowledge that matters but isn't frequently retrieved (e.g., "Chris's birthday").
- **Surface**: Request the engine to highlight tensions, thin areas, and open loops for attention.

## Engine-Surfaced Attention

The engine should proactively identify areas that benefit from curation:
- **Tensions**: Beliefs that contradict each other
- **Thin areas**: Topics with few beliefs or low confidence
- **Open loops**: Threads or questions that were started but never resolved
- **Stale clusters**: High-connection areas that haven't been retrieved recently

## Content-Significant vs Pattern-Significant

Not all ingested data has content significance. The engine distinguishes:

### Content-Significant
Full nodes with embeddings, clustering, and graph participation. Examples: conversations, decisions, insights, code changes.

### Pattern-Significant
Statistical fingerprints only — counts, frequencies, timing patterns. No full content storage. Examples: email frequency by sender, meeting cadence, notification patterns. The signal isn't in the content but in the metadata patterns. Useful for the human-agent pair to tune external systems.

## Open Questions

- How do explicit corrections interact with organic signals? Does a correction reset decay?
- Should surfaced attention items have their own priority queue or integrate into normal retrieval?
- What's the minimal statistical fingerprint format for pattern-significant data?
- How does curation interact with federation? Can curated corrections propagate?
