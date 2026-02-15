# Privacy & Data Sovereignty

**Status**: settled  
**Confidence**: 0.90  
**Connections**: [intermediary], [federation], [knowledge-lifecycle], [epistemics-native]

## Idea

Privacy and sovereignty are structural properties of the engine, not features bolted on top. The architecture makes data leakage structurally impossible for certain classes of data.

## Sovereignty by Layer

- **L0 data never shares.** Raw ingestion is private by definition. Emails, private messages, sensitive documents — they never leave the device.
- **L1-L2 sharing requires explicit intent.** Share a *belief*, not the raw data it came from. "Chris prefers composable architectures" without the private conversations that produced it.
- **L3-L4 is where federation gets interesting.** High-confidence, established beliefs are candidates for cross-agent corroboration. Knowledge building, not data sharing.

## The Provenance Firewall

Every belief knows where it came from (which ingestion events, which sessions). But raw provenance is private. What gets shared:
- The belief content
- Its confidence score
- The *fact* that it has provenance (not the provenance itself)
- A reputation signal about the source

A receiving node can say "this belief came from a source with reputation X" without knowing which conversations produced it.

## Encryption

- At-rest encryption by default in the Rust engine
- Federation uses DID key exchange
- The application layer (OpenClaw, the agent) never sees raw data from another node — only decrypted beliefs that were explicitly shared

## Sharing Intents (from Valence)

- **know_me**: Private 1:1 sharing
- **work_with_me**: Bounded group sharing
- **learn_from_me**: Cascading (can be reshared)
- **use_this**: Public

## The Engine Is Always Local

Even in a federation scenario, the engine runs on your device. Federation is about sharing selected beliefs between local engines — not about centralizing data. There is no cloud. There is no server that sees everything. Each node is sovereign.

## Open Questions

- How do we handle beliefs derived from shared beliefs? (Transitive provenance)
- Can a shared belief be "recalled" after sharing? (Revocation semantics)
- How does encryption interact with the lazy compute model? (Encrypting L0 vs L3)
- What about legal compliance (GDPR right to deletion) across federated beliefs?
