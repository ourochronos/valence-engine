# Federation

**Status**: exploring  
**Confidence**: 0.70  
**Connections**: [privacy-sovereignty], [epistemics-native], [pekb], [trust-verification]

## Idea

Local engines share selected beliefs with each other through a P2P protocol. No central server. Each node is sovereign. Knowledge builds through cross-node corroboration.

## What Gets Shared

- Beliefs (not raw data, not tool results, not conversation history)
- Confidence scores
- The fact of provenance (not the provenance itself)
- Reputation signals

## What Never Gets Shared

- L0 raw ingestion data
- Conversation transcripts
- Tool results
- Working set / session state
- The graph structure itself (only individual beliefs traverse)

## Nested Epistemic States (PEKB Connection)

When Node A shares a belief with Node B:
- Node B has a belief: "Node A believes X with confidence 0.8"
- This is a nested epistemic state — belief about a belief
- If Node B independently arrives at X, that's corroboration → both nodes' confidence increases
- If Node B has a contradicting belief, that's a cross-node tension

## Corroboration Protocol

1. Node A shares belief X
2. Node B checks for similar beliefs (semantic similarity ≥ threshold)
3. If found: corroboration event. Both beliefs get confidence boost.
4. If contradicted: cross-node tension. Both nodes are informed.
5. If novel: Node B stores as "belief from A" at lower initial confidence

## Trust Propagation

- Nodes build reputation through accurate sharing
- Corroborated beliefs boost the sharer's reputation
- Contradicted beliefs (that turn out to be wrong) lower reputation
- Reputation affects how future shared beliefs are weighted

## Open Questions

- Discovery: how do nodes find each other? (DID exchange, manual pairing, DHT?)
- Latency: sync protocol — push, pull, or gossip?
- Versioning: what happens when a shared belief is superseded on the source?
- Scale: federation of 2 nodes vs 200 — same protocol?
- Incentives: why would a node share? (reputation gains, reciprocal knowledge)
