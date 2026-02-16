# Self-Training Boundary Models

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [knowledge-loop], [cold-warm-split], [inference-training-loop], [graceful-degradation]

## Core Insight

**The LLM at the decomposition/recomposition boundary generates its own training data.**

Every interaction creates a training pair:
- **Decomposition**: Natural language → triples (input: NL, output: structured triples)
- **Recomposition**: Triples → natural language (input: structured triples, output: NL)

Store these pairs. Use them to train small, specialized models. Replace the expensive LLM boundary with cheap local inference.

The scaffolding falls away as the system matures.

## The Progression

### Phase 1: LLM Everywhere (Cold Start)
- **Decomposition**: Claude/GPT-4 converts "Chris prefers composable architectures" → triples
- **Recomposition**: Claude/GPT-4 converts triples → "Based on 5 sources, Chris strongly favors..."
- **Cost**: $$ per query (API calls)
- **Latency**: 500-2000ms (network round-trip)
- **Quality**: Excellent (frontier models)

**Why start here**: Bootstrap quickly, high quality, no ML expertise required.

### Phase 2: Fine-Tuned Small Model (1-3B params, MLX)
After collecting 1000+ training pairs:
- Fine-tune small model (Phi-3, Llama 3.2 1B, Qwen 2.5 1.5B)
- Run locally via MLX (Apple Silicon) or ONNX Runtime (cross-platform)
- **Decomposition**: 95% of cases handled by local model, 5% fall back to LLM
- **Recomposition**: 90% handled locally (structured → NL is easier)
- **Cost**: $ per query (mostly free, occasional LLM fallback)
- **Latency**: 50-200ms (local inference)
- **Quality**: Very good (fine-tuned on domain-specific data)

**Why transition here**: Proven task, narrow domain, small models excel at narrow tasks.

### Phase 3: Specialized Encoder (Later Optimization)
After collecting 10K+ training pairs:
- Train specialized encoder (decomposition is NER + relation extraction — narrow task)
- **Decomposition**: 99% handled by encoder, 1% LLM fallback (rare edge cases)
- **Recomposition**: Still use fine-tuned model (generation is harder than extraction)
- **Cost**: Free (no API calls)
- **Latency**: 10-50ms (fast inference)
- **Quality**: Excellent on domain, degrades gracefully on out-of-domain

**Why eventually**: Maximum efficiency, offline-first, no API dependency.

## Why This Works

### Decomposition is Narrow
Converting natural language to triples is essentially:
1. **Named Entity Recognition**: Identify subjects and objects ("Chris", "composable architectures")
2. **Relation Extraction**: Identify the relationship ("prefers")
3. **Normalization**: Canonicalize entities (typos, synonyms)

This is a **well-studied, narrow task**. Small models (1-3B params) can achieve 95%+ accuracy when fine-tuned on domain data.

### The Training Data is Generated Automatically
No manual labeling required:
- LLM converts NL → triples (during normal operation)
- Store the pair: `(input: "Chris prefers X", output: [(Chris, prefers, X)])`
- Accumulate thousands of examples passively
- Fine-tune small model on these examples

**The system trains itself through normal use.**

### Small Models Excel at Narrow Tasks
Frontier LLMs (Claude Opus, GPT-4) are generalists. They're overkill for:
- "Extract entities and relations from this sentence"
- "Generate a natural language summary from these triples"

Small models (1-3B params) fine-tuned on specific tasks often **match or exceed** frontier model performance on that task, because:
- All capacity focused on one task (no wasted parameters)
- Training data is perfectly aligned (domain-specific examples)
- Inference is 10-100x faster (fewer parameters to activate)

### Graceful Fallback
The fine-tuned model doesn't have to be perfect:
- 95% accuracy → handle locally
- 5% edge cases → fall back to LLM
- User never sees the difference (transparent fallback)

Over time, as the training dataset grows, accuracy improves → fewer fallbacks.

## Implementation Strategy

### Data Collection (Automatic)
Every decomposition/recomposition call:
```rust
struct BoundaryCall {
    input: String,           // Natural language or triples
    output: String,          // Triples or natural language
    direction: Direction,    // Decomposition or Recomposition
    timestamp: DateTime,
    llm_model: String,       // Which model generated this
    quality_score: f32,      // Optional: user feedback or validation
}
```

Store in Postgres. Accumulate passively.

### Training Trigger (Automatic or Manual)
**Automatic**: After collecting 1000 new examples, trigger fine-tuning job  
**Manual**: User runs `valence train-boundary` when desired

### Fine-Tuning (MLX for Apple Silicon)
```python
import mlx.nn as nn
from mlx.utils import tree_flatten
import mlx.optimizers as optim

# Load base model (Phi-3, Llama, Qwen)
model = load_model("microsoft/Phi-3-mini-4k-instruct")

# Load training data
train_data = load_boundary_calls(direction="decomposition", min_examples=1000)

# Fine-tune (LoRA for efficiency)
trainer = LoRATrainer(model, rank=16, alpha=32)
trainer.train(train_data, epochs=3, batch_size=4)

# Save fine-tuned model
trainer.save("~/.valence/models/boundary-decomposition-v1")
```

**Result**: 1.5GB model file, runs locally, <100ms inference.

### Inference (Local-First, LLM Fallback)
```rust
fn decompose(text: &str) -> Result<Vec<Triple>, Error> {
    // Try local model first
    match local_model.decompose(text) {
        Ok(triples) if confidence >= 0.90 => Ok(triples),
        _ => {
            // Fall back to LLM
            let triples = llm.decompose(text)?;
            
            // Store this call as training data
            store_boundary_call(text, triples, Direction::Decomposition);
            
            Ok(triples)
        }
    }
}
```

### Quality Monitoring (Continuous)
Track metrics:
- **Fallback rate**: How often do we use LLM vs local model?
- **Latency**: Average inference time (local vs LLM)
- **Cost**: API spend (should trend toward $0)
- **Accuracy**: User corrections / total calls (measure quality degradation)

**Goal**: Fallback rate < 5%, latency < 100ms, cost near $0, accuracy > 95%.

## Same Pattern as Topology Embeddings

This is the **same bootstrap strategy** as topology embeddings:

### Topology Embeddings
1. **Phase 1**: Use external LLM embeddings (fast, high-quality, API-dependent)
2. **Phase 2**: Blend LLM + topology embeddings (graph is growing)
3. **Phase 3**: Pure topology embeddings (graph is mature, no API dependency)

### Boundary Models
1. **Phase 1**: Use frontier LLM for decomposition/recomposition (fast, high-quality, API-dependent)
2. **Phase 2**: Fine-tuned small model + LLM fallback (training data accumulating)
3. **Phase 3**: Specialized encoder for decomposition (training data is large, no API dependency)

**In both cases**: Start with expensive scaffolding that works immediately. Use it to generate data/structure. Train local replacement. Transition to local. Scaffolding falls away.

## Why This Matters

### Cost Reduction
- Phase 1: $0.10 per 100 decomposition calls (GPT-4 API)
- Phase 2: $0.01 per 100 calls (mostly local, rare fallback)
- Phase 3: $0.00 per 100 calls (fully local)

**For a user with 10K queries/month**: $100/mo → $10/mo → $0/mo

### Privacy
- Phase 1: Every query sent to external API (privacy leak)
- Phase 2: 95% local, 5% external
- Phase 3: 100% local (zero external data sharing)

**For privacy-sensitive users**: This is the path to fully offline operation.

### Latency
- Phase 1: 500-2000ms (network + API processing)
- Phase 2: 50-200ms (local inference)
- Phase 3: 10-50ms (optimized encoder)

**For real-time use cases**: 10ms decomposition enables sub-100ms context assembly.

### Offline Operation
- Phase 1: Requires internet + API key
- Phase 2: Mostly offline, degrades gracefully without internet
- Phase 3: Fully offline (no external dependency)

**For mobile/edge/air-gapped deployments**: This is required.

## Open Questions

- What's the minimum training data size for fine-tuning? (1000 examples? 5000?)
- Which base model works best? (Phi-3, Llama 3.2, Qwen 2.5, Mistral?)
- Should we use LoRA or full fine-tuning? (LoRA is faster, full is more flexible)
- How do we handle model updates? (new base model released → retrain boundary model?)
- Can we share trained boundary models across users? (privacy implications)
- Should decomposition and recomposition use the same model or separate models?
- How do we measure quality degradation? (user corrections? held-out test set?)

## Related Concepts

- [knowledge-loop]: Boundary models are part of the self-reinforcing loop
- [cold-warm-split]: LLM is at boundary only, engine is deterministic
- [inference-training-loop]: Usage generates training data → trains model → improves usage
- [graceful-degradation]: Local model enables offline operation, LLM fallback ensures quality
