# Graceful Degradation

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [cold-warm-split], [lazy-compute], [privacy-sovereignty]

## Idea

The system works without inference at lower resolution. Inference makes it richer but is never a dependency. Graceful degradation is a structural property, not a fallback mode.

## Degradation Levels

### Full Mode (Cold + Warm)
- Context assembly with synthesis
- Intent recognition and tool mediation
- Label generation for clusters
- Confidence dimension naming
- Pattern detection and L4 insights
- All features, best experience

### Cold Mode (Deterministic Only)
- Context assembly with pure fusion (no synthesis)
- Retrieval based on embeddings + graph + confidence
- Decay and eviction work normally
- Knowledge lifecycle progresses through use
- No LLM calls, no inference costs
- **Fully functional, just less refined**

### Offline Mode
- Local embeddings only (pre-computed or quantized BERT in binary)
- No external API calls of any kind
- Works on airplane, in bunker, on Mars
- Privacy-absolute deployment

### Minimal Mode
- Graph traversal and recency only (no embeddings)
- Keyword-based retrieval
- Still better than grep

## Why This Matters

1. **Cost control**: Run cold during batch operations, warm during interactive use
2. **Privacy**: Full function without sending data to any API
3. **Reliability**: System doesn't break when API is down or budget is exhausted
4. **Portability**: Can deploy in constrained environments (edge devices, air-gapped systems)
5. **Trust**: Users know the system isn't dependent on external services

## Performance Characteristics

| Mode | Latency | Cost | Quality |
|------|---------|------|---------|
| Full | 50-200ms | $$$ | Best |
| Cold | 20-50ms | $ (compute only) | Good |
| Offline | 20-50ms | Free (after setup) | Good |
| Minimal | <10ms | Free | Acceptable |

## Configuration

Degradation can be:
- **Automatic**: System detects API failure and falls back
- **Scheduled**: Warm mode during work hours, cold mode overnight
- **Budget-based**: Switch to cold when inference budget is exhausted
- **Manual**: User explicitly chooses mode

## What Requires Inference (Warm Only)

- L4 synthesis (combining beliefs into new insights)
- Entity type labeling (deriving types from usage patterns)
- Intent recognition (understanding *why* a query was made)
- Conflict resolution suggestions (how to resolve tensions)
- Custom dimension naming (what to call domain-specific confidence dimensions)

Everything else is deterministic and works in cold mode.

## Cold Mode Example

Query: "What do I know about Valence architecture?"

**Cold mode response**:
- Retrieves top 10 beliefs by semantic similarity to "Valence architecture"
- Ranks by confidence × recency × graph proximity to "Valence" entity
- Returns structured list: beliefs + confidence scores + timestamps
- LLM formats as natural language (read-only, no expensive reasoning)

**Warm mode would add**:
- Synthesis of beliefs into coherent narrative
- Detection of gaps ("You have beliefs about federation but no beliefs about deployment")
- Pattern recognition ("These beliefs cluster around 'epistemic-native' concept")
- Intent-based filtering (if query implies concern about scalability, surface those beliefs)

Both work. Warm is just better.

## Open Questions

- Should degradation be user-visible? ("Now running in cold mode to conserve budget")
- Can we measure the quality delta between cold and warm empirically?
- Should there be hybrid modes? (e.g., warm synthesis but cold retrieval)
- How do we test that cold mode truly works without inference?
