# Concept Edges

Relationships between concept nodes.

## Core Architecture Cluster
```
context-assembly ←→ working-set          (assembles from)
context-assembly ←→ multi-dim-fusion     (powered by)
context-assembly ←→ tool-mediation       (enriched by)
context-assembly ←→ decay-model          (replaces compaction via)
context-assembly ←→ intermediary         (implemented by)
```

## Knowledge Substrate Cluster
```
epistemics-native ←→ multi-dim-fusion    (dimensions feed)
epistemics-native ←→ knowledge-lifecycle (lifecycle of)
epistemics-native ←→ federation          (enables)
knowledge-lifecycle ←→ progressive-summarization (follows)
knowledge-lifecycle ←→ lazy-compute      (driven by)
knowledge-lifecycle ←→ decay-model       (decay rules)
```

## Emergence Cluster
```
stigmergy ←→ progressive-summarization   (dual principles)
stigmergy ←→ emergent-ontology           (structure emerges via)
stigmergy ←→ lazy-compute                (compute follows use via)
stigmergy ←→ decay-model                 (relevance emerges via)
emergent-ontology ←→ knowledge-lifecycle (entities emerge through)
```

## Tool & Intent Cluster
```
tool-mediation ←→ intent-capture         (wraps with)
tool-mediation ←→ intermediary           (implemented by)
tool-mediation ←→ context-assembly       (enriches)
tool-mediation ←→ stigmergy              (hygiene through)
intent-capture ←→ working-set            (informs)
```

## Implementation Cluster
```
rust-engine ←→ multi-dim-fusion          (implements)
rust-engine ←→ lazy-compute              (enables)
rust-engine ←→ epistemics-native         (native in)
rust-engine ←→ intermediary              (is the)
```

## Trust & Sharing Cluster
```
federation ←→ privacy-sovereignty        (constrained by)
federation ←→ epistemics-native          (nested beliefs via)
privacy-sovereignty ←→ intermediary      (enforced by)
privacy-sovereignty ←→ knowledge-lifecycle (L0 never shares)
```

## Core Principles Cluster
```
cold-warm-split ←→ graceful-degradation    (enables)
cold-warm-split ←→ lazy-compute            (deterministic layer)
cold-warm-split ←→ epistemics-native       (cold engine core)
inference-training-loop ←→ stigmergy       (use reshapes structure)
inference-training-loop ←→ lazy-compute    (compute follows use)
inference-training-loop ←→ knowledge-lifecycle (drives progression)
value-per-token ←→ lazy-compute            (maximized by)
value-per-token ←→ context-assembly        (drives design)
value-per-token ←→ tool-mediation          (extract not accumulate)
```

## Memory Management Cluster
```
bounded-memory ←→ decay-model              (implements via LRU)
bounded-memory ←→ inference-training-loop  (eviction is forgetting)
bounded-memory ←→ knowledge-lifecycle      (natural narrowing)
bounded-memory ←→ stigmergy                (LRU eviction is forgetting)
bounded-memory ←→ working-set              (residency competition)
bounded-memory ←→ deterministic-core       (eviction is deterministic)
bounded-memory ←→ merge-model              (clustering preserves before eviction)
bounded-memory ←→ curation                 (pins protect from eviction)
graceful-degradation ←→ privacy-sovereignty (enables offline)
graceful-degradation ←→ cold-warm-split    (structural property)
```

## Deterministic Core Cluster
```
deterministic-core ←→ rust-engine        (implemented in)
deterministic-core ←→ stigmergy          (makes concrete)
deterministic-core ←→ lazy-compute       (compute is deterministic bookkeeping)
deterministic-core ←→ curation           (warm engine layer)
deterministic-core ←→ bounded-memory     (eviction is deterministic)
deterministic-core ←→ merge-model        (clustering is deterministic)
```

## Emergent Dimensions Cluster
```
emergent-dimensions ←→ multi-dim-fusion     (extends dimensions of)
emergent-dimensions ←→ emergent-ontology    (dimensions emerge like ontology)
emergent-dimensions ←→ stigmergy            (use creates dimensions)
emergent-dimensions ←→ epistemics-native    (dimensional space is epistemic)
emergent-dimensions ←→ topology-embeddings  (computed from topology)
emergent-dimensions ←→ graph-vector-duality (topology IS dimensional space)
emergent-dimensions ←→ three-layer-architecture (confidence from source topology)
```

## Curation Cluster
```
curation ←→ deterministic-core           (warm engine enrichment)
curation ←→ tool-mediation               (curation tools are mediated)
curation ←→ epistemics-native            (deliberate epistemic correction)
curation ←→ bounded-memory               (pins protect from eviction)
```

## Merge Model Cluster
```
merge-model ←→ bounded-memory            (eviction after clustering)
merge-model ←→ deterministic-core        (clustering is deterministic)
merge-model ←→ progressive-summarization (clustering IS progressive summarization)
merge-model ←→ stigmergy                 (co-retrieval drives merging)
merge-model ←→ lazy-compute              (merge is demand-driven)
merge-model ←→ knowledge-lifecycle       (L1-L2 consolidation)
```

## Atomic Knowledge Cluster (NEW - Feb 15, 2026)
```
triples-atomic ←→ three-layer-architecture    (foundation of)
triples-atomic ←→ emergent-ontology           (ontology built from)
triples-atomic ←→ epistemics-native           (confidence on)
three-layer-architecture ←→ progressive-summarization (implements)
three-layer-architecture ←→ knowledge-lifecycle (structures)
```

## Topology & Vector Cluster (NEW - Feb 15, 2026)
```
topology-embeddings ←→ graph-vector-duality   (enables)
topology-embeddings ←→ stigmergy              (refines through use)
topology-embeddings ←→ lazy-compute           (computed on demand)
graph-vector-duality ←→ multi-dim-fusion      (retrieval via)
graph-vector-duality ←→ emergent-ontology     (discovers structure)
```

## Complete Loop Cluster (NEW - Feb 15, 2026)
```
knowledge-loop ←→ inference-training-loop     (realizes)
knowledge-loop ←→ topology-embeddings         (powered by)
knowledge-loop ←→ graph-vector-duality        (relies on)
knowledge-loop ←→ triples-atomic              (builds from)
knowledge-loop ←→ cold-warm-split             (LLM at boundary)
knowledge-loop ←→ stigmergy                   (self-reinforcing via)
```

## Integration Edges (NEW connects to EXISTING)
```
triples-atomic ←→ knowledge-lifecycle         (lifecycle of)
triples-atomic ←→ bounded-memory              (dedup at atomic layer)
three-layer-architecture ←→ context-assembly  (retrieval from L3)
topology-embeddings ←→ rust-engine            (implemented in)
graph-vector-duality ←→ federation            (shares triples not vectors)
knowledge-loop ←→ value-per-token             (maximizes via loop)
```

## PostgreSQL + Rust Architecture Cluster (NEW - Feb 15, 2026)
```
postgres-rust-architecture ←→ rust-engine             (implements)
postgres-rust-architecture ←→ triples-atomic          (stores)
postgres-rust-architecture ←→ topology-embeddings     (computes in Rust)
postgres-rust-architecture ←→ graph-vector-duality    (Rust holds both)
postgres-rust-architecture ←→ deterministic-core      (Rust is compute)
```

## Budget-Bounded Operations Cluster (NEW - Feb 15, 2026)
```
budget-bounded-ops ←→ lazy-compute                    (extreme laziness)
budget-bounded-ops ←→ value-per-token                 (forces value maximization)
budget-bounded-ops ←→ graceful-degradation            (enables via budgets)
budget-bounded-ops ←→ context-assembly                (primary application)
budget-bounded-ops ←→ multi-dim-fusion                (bounded fusion queries)
```

## Self-Training Boundary Cluster (NEW - Feb 15, 2026)
```
self-training-boundary ←→ knowledge-loop              (part of loop)
self-training-boundary ←→ cold-warm-split             (LLM at boundary)
self-training-boundary ←→ inference-training-loop     (boundary trains itself)
self-training-boundary ←→ graceful-degradation        (local model enables offline)
self-training-boundary ←→ self-closing-loops          (Loop 3)
```

## Engine + Network Product Cluster (NEW - Feb 15, 2026)
```
engine-network-product ←→ federation                  (network layer)
engine-network-product ←→ rust-engine                 (engine is embeddable)
engine-network-product ←→ privacy-sovereignty         (preserved in both)
engine-network-product ←→ epistemics-native           (both layers are epistemic)
engine-network-product ←→ network-flows               (what flows through network)
```

## Network Flows Cluster (NEW - Feb 15, 2026)
```
network-flows ←→ engine-network-product               (content of network)
network-flows ←→ federation                           (how flows happen)
network-flows ←→ self-training-boundary               (models/data flow)
network-flows ←→ knowledge-loop                       (loop extends to network)
network-flows ←→ triples-atomic                       (triples are what flows)
```

## Emergence Through Composition Cluster (NEW - Feb 15, 2026)
```
emergence-composition ←→ stigmergy                    (same philosophy)
emergence-composition ←→ emergent-ontology            (ontology emerges)
emergence-composition ←→ inference-training-loop      (emergence through use)
emergence-composition ←→ engine-network-product       (network emergence)
emergence-composition ←→ self-closing-loops           (loops emerge)
```

## Self-Closing Loops Cluster (NEW - Feb 15, 2026)
```
self-closing-loops ←→ knowledge-loop                  (Loop 1: graph→vectors)
self-closing-loops ←→ topology-embeddings             (generates embeddings)
self-closing-loops ←→ stigmergy                       (Loop 2: usage→structure)
self-closing-loops ←→ self-training-boundary          (Loop 3: system→boundary)
self-closing-loops ←→ inference-training-loop         (self-improvement pattern)
```

## Key Insight Edges
```
stigmergy + progressive-summarization = "structure emerges from use"
context-assembly + decay-model = "no more compaction"
tool-mediation + intent-capture = "tools get smarter over time"
intermediary = rust-engine = "the engine IS the intermediary"
deterministic-core + stigmergy = "stigmergy made concrete"
cold-warm-split + graceful-degradation = "inference is optional, not required"
inference-training-loop + stigmergy = "usage is the training process"
bounded-memory + decay-model = "eviction is natural forgetting"
lazy-compute + deterministic-core = "only deterministic bookkeeping, not inference"
merge-model + progressive-summarization = "clustering IS summarization"
value-per-token = "the primary optimization metric"
triples-atomic = "the atomic unit of knowledge"
three-layer-architecture = "triples → sources → summaries (not three tables, three scales)"
topology-embeddings = "vectors from graph structure alone, no external model"
graph-vector-duality = "two projections of one system"
knowledge-loop = "LLM builds graph → topology generates embeddings → retrieval feeds LLM"
postgres-rust-architecture = "PostgreSQL is persistence, Rust is compute"
budget-bounded-ops = "good-enough beats perfect when read boundary is fuzzy"
self-training-boundary = "LLM generates training data for its own replacement"
engine-network-product = "engine is the brain, Valence is the nervous system"
network-flows = "everything that decomposes to triples flows through network"
emergence-composition = "vision emerges through iteration, not specification"
self-closing-loops = "scaffolding that trains its own replacement"
```
