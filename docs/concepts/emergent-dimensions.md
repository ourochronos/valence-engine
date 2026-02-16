# Emergent Dimensions

**Status**: exploring  
**Confidence**: 0.75  
**Connections**: [multi-dim-fusion], [emergent-ontology], [stigmergy], [epistemics-native]

## Idea

Don't predefine the dimension space. The first belief has zero dimensions beyond content and timestamp. When a second similar belief arrives, the axis between them IS a dimension. Clusters create dimensions. Usage patterns create dimensions. The dimensional space is literally the shape of what you've cared about.

## How Dimensions Emerge

1. **Single node**: No dimensions. Just content + timestamp.
2. **Two similar nodes**: The axis of similarity between them is the first dimension.
3. **Cluster forms**: The cluster's shape defines a local dimensional subspace.
4. **Usage patterns**: Repeated co-retrieval along a particular axis strengthens that dimension. Neglected axes fade.
5. **A scalar** is just a space where you've only ever needed one axis.

## Relationship to Multi-Dim Fusion

[multi-dim-fusion] defines how to score across multiple dimensions simultaneously. Emergent dimensions extends this: the dimensions themselves aren't predefined (semantic, temporal, confidence, etc.) — they grow from the data and its use. The scoring function adapts as new dimensions appear from usage patterns.

Some dimensions may correspond to the predefined ones (recency, confidence). Others may be entirely novel — axes that only exist because of what this particular user has cared about.

## What This Replaces

Traditional: define schema → fit data to schema → query within schema.  
Emergent: data arrives → use creates structure → dimensions are discovered → schema is the shape of use.

## Open Questions

- How are structural dimensions represented internally? Named vectors? Unlabeled axes?
- Does naming a dimension via inference change its behavior? (Does the label affect retrieval?)
- How do dimensions merge or split as the graph evolves?
- What's the minimum cluster size before a dimension is "real"?
