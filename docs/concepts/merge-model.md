# Merge Model

**Status**: exploring  
**Confidence**: 0.80  
**Connections**: [bounded-memory], [deterministic-core], [progressive-summarization], [stigmergy], [lazy-compute]

## Idea

Merge doesn't mean "synthesize two sentences into one" — that requires inference. Merge means clustering: related nodes become one cluster node with the most-retrieved member as representative. Individual nodes drop below retrieval threshold and get evicted by LRU. The cluster inherits the combined dimensional profile. Pure deterministic bookkeeping.

## How Clustering Works

1. **Co-retrieval counting**: When two nodes are pulled together repeatedly, that's a structural link. Co-retrieval count is the primary signal.
2. **Cluster formation**: Nodes with high co-retrieval counts form a cluster. The cluster is a new node in the graph.
3. **Representative selection**: The most-retrieved member becomes the cluster representative — the face of the cluster in retrieval results.
4. **Individual eviction**: Once clustered, individual nodes compete for LRU residency on their own. Low-retrieval members fade and get evicted. The cluster retains their dimensional contribution.
5. **Dimensional inheritance**: The cluster's dimensional profile is the union of its members' profiles.

## Inference as Optional Enrichment

When the warm engine ([deterministic-core]) interacts with a cluster, it can optionally leave behind a synthesis — a generated summary that captures the cluster's meaning. But the system doesn't depend on it. The cluster works fine as "these N things that keep getting retrieved together, represented by the most-used one."

## What This Replaces

Traditional summarization: take N documents → run inference → produce summary → replace originals.  
Merge model: co-retrieval creates clusters → most-used member represents → originals evict naturally → inference enriches optionally.

## Open Questions

- Is the representative the most-retrieved member, or the most-connected? Does it matter?
- If inference generates a synthesis, does the synthesis become the new representative?
- When a cluster member is evicted, how is its dimensional contribution preserved? Snapshot at eviction time?
- Can clusters nest? (Cluster of clusters as the graph matures)
