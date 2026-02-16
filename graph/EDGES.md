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

## Bounded Memory Cluster
```
bounded-memory ←→ decay-model            (decay feeds eviction)
bounded-memory ←→ stigmergy              (LRU eviction is forgetting)
bounded-memory ←→ knowledge-lifecycle    (bounds the lifecycle)
bounded-memory ←→ working-set            (residency competition)
bounded-memory ←→ deterministic-core     (eviction is deterministic)
bounded-memory ←→ merge-model            (clustering preserves before eviction)
bounded-memory ←→ curation               (pins protect from eviction)
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
emergent-dimensions ←→ multi-dim-fusion  (extends dimensions of)
emergent-dimensions ←→ emergent-ontology (dimensions emerge like ontology)
emergent-dimensions ←→ stigmergy         (use creates dimensions)
emergent-dimensions ←→ epistemics-native (dimensional space is epistemic)
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

## Key Insight Edges
```
stigmergy + progressive-summarization = "structure emerges from use"
context-assembly + decay-model = "no more compaction"
tool-mediation + intent-capture = "tools get smarter over time"
intermediary = rust-engine = "the engine IS the intermediary"
deterministic-core + stigmergy = "stigmergy made concrete"
bounded-memory + decay-model = "forgetting is structural, not cleanup"
lazy-compute + deterministic-core = "only deterministic bookkeeping, not inference"
merge-model + progressive-summarization = "clustering IS summarization"
```
