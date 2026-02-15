# Progressive Summarization

**Status**: settled  
**Confidence**: 0.95  
**Connections**: [stigmergy], [lazy-compute], [decay-model], [knowledge-lifecycle]

## Idea

Knowledge progresses through layers of refinement, driven by use. Adapted from Tiago Forte's model but applied to machine epistemics rather than human note-taking.

## Layers

- **L0**: Raw ingestion. Text + timestamp + source + cheap fingerprint (trigram hash). Most data lives here permanently. Near-zero compute cost.
- **L1**: First retrieval. Something asked for this data. Now it gets a real embedding, maybe entity extraction. Becomes a proper belief with initial confidence. First real compute spend.
- **L2**: Repeated retrieval / corroboration. Multiple queries have pulled this up, or another ingestion produced something similar. Gets graph connections, refined confidence, possible supersession of earlier versions.
- **L3**: Established knowledge. High confidence, well-connected, corroborated. Eligible for auto-recall during context assembly. The system is *confident* about this.
- **L4**: Synthesized insight. Agent or system combines multiple L3 beliefs into new understanding. "Chris prefers composable architectures" — synthesized from dozens of observations.

## Critical Property

**Compute follows attention, not ingestion.** Don't run BERT on every email. Store raw text cheaply. When retrieval touches it, THEN embed. The system is lazy by design — it only spends compute where use has demonstrated value.

## Volume Solution

1000 messages/day. Maybe 10 matter. You don't know which 10 at ingestion time. Within a week, retrieval patterns make it obvious. The 10 that got pulled into context got promoted to L1. The rest stay at L0, cheap and quiet.
