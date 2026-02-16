# Knowledge Lifecycle

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [progressive-summarization], [lazy-compute], [stigmergy], [epistemics-native], [decay-model], [bounded-memory], [merge-model]

## Idea

Data flows through a lifecycle from raw ingestion to established knowledge, driven by use rather than explicit curation. The lifecycle IS the progressive summarization model applied to machine epistemics.

## The Flow

```
Raw data (any source)
  → L0: Cheap store (text + timestamp + source + fingerprint)
      ↓ (on first retrieval)
  → L1: Promoted (embedding, entity extraction, belief creation)
      ↓ (on repeated use)
  → L2: Refined (graph connections, confidence updates, supersession)
      ↓ (on corroboration)
  → L3: Established (high confidence, auto-recall eligible)
      ↓ (on synthesis)
  → L4: Insight (cross-belief synthesis, new understanding)
```

## Data Sources (All Enter at L0)

- Conversations (messages, chat history)
- Email
- Code commits / diffs
- Calendar events
- Documents (PDFs, notes, files)
- Web browsing (fetched pages)
- Tool results
- Sensor data (location, photos)
- Other agents' shared beliefs (enter at L1 with source reputation weighting)

## Decay

- L0 data that's never retrieved → ages naturally, cheap storage cost
- L1+ beliefs that stop being retrieved → confidence slowly decays
- Superseded beliefs → retained in chain but deprioritized in fusion
- Archived beliefs → kept for provenance but excluded from retrieval
- Explicit forget → GDPR-compliant removal from all layers

## Promotion Triggers

- **L0 → L1**: First retrieval by a query
- **L1 → L2**: Multiple retrievals, or corroboration from another ingestion
- **L2 → L3**: High confidence + well-connected in graph + corroborated
- **L3 → L4**: Agent synthesis (explicit) or engine inference (detected pattern across beliefs)

## What This Replaces

Traditional: ingestion → batch process → index → retrieve
This: ingestion → cheap store → use-driven promotion → organic refinement

## Bounded Lifecycle

The lifecycle is bounded by [bounded-memory]. L0 data that never promotes eventually gets evicted from the LRU. The boundary ensures the lifecycle can't accumulate unbounded L0 — there's a hard cap on total nodes, and unpromoted L0 data is the most likely to be evicted (lowest retrieval frequency, fewest connections).

Merge/clustering ([merge-model]) is how L1-L2 beliefs consolidate without inference. Co-retrieved beliefs cluster together, the most-used member becomes the representative, and individual members that stop being retrieved get evicted naturally. This is progressive summarization implemented as deterministic bookkeeping.

## Known Issues (from current Valence KB)

### Single-Inference Beliefs Propagate as Truth
Problem: One conversation infers "Chris doesn't like frameworks" and it carries forward across sessions as established philosophy. Beliefs from single interactions get stored with high confidence.

**Root cause**: No corroboration tracking after initial creation. The 6D confidence model has a corroboration dimension but it's never updated.

**Solution needed**: 
- Beliefs from single interactions should start at LOW confidence (0.3-0.5)
- Confidence increases only through independent corroboration (3+ sessions observing same pattern)
- Same user saying it twice in one session ≠ corroboration
- Different sessions = corroboration

### No Automatic Deduplication
Problem: beliefs table has content_hash column but never checks it for uniqueness. Similar beliefs across sessions accumulate as duplicates rather than reinforcing confidence.

**Solution needed**: 
- Exact content_hash check on belief_create (fast, catches exact dupes)
- Embedding similarity > 0.90 check for fuzzy matches
- On match: increment corroboration count + escalate confidence instead of creating duplicate

### Corroboration Dimension Never Updates
The confidence model has `corroboration` as a dimension but it's set once at creation and never updated as beliefs are independently confirmed.

**Solution needed**: 
- Track corroboration count (how many independent sessions contributed)
- Track source diversity (same user ≠ diverse, different sessions = diverse)
- Auto-update corroboration dimension when similar beliefs arrive from independent sources

## Open Questions

- What triggers demotion? (Confidence drops below threshold? Contradicted by newer evidence?)
- How do we handle data sources that need immediate processing? (e.g., urgent messages)
- Should L4 synthesis be agent-driven only, or can the engine infer insights autonomously?
- What's the storage cost model? (L0 should be nearly free, L3 can be more expensive)
- What's the minimum corroboration count before confidence escalates? (3 sessions? 5?)
- How long is the window for "independent" sessions? (Same day = not independent? Different weeks = independent?)
