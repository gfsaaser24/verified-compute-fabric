# Changes from version 1.0

Version 1.0 (24 October 2026) was drafted with ChatGPT.
Version 2.0 keeps its thesis, its two profiles, its failure semantics, its acceptance tests and all 40 of its references.
The changes below are additions or corrections.

| Area | Change |
|---|---|
| Structure | Regrouped into five parts. Twelve figures and two pieces of generative artwork added. Caveats that v1.0 repeated in most paragraphs are stated once, in the front matter, and the prose is shorter and plainer. |
| [§1](01-why-breaches-spread.md) Problem | New. Seven verified public incidents, each mapped to the boundary it shows. New exposure model, with the distinction between a stolen credential and a controlled workload. |
| [§2](02-scope-and-threat-model.md) Threat model | Four new threat rows: validly signed malicious dependency, prompt-injected agent, stolen issuer key, physical memory access. |
| [§5](05-supply-chain.md) Supply chain | Transparency logging of approvals. Publishing without long-lived tokens. SolarWinds, xz-utils, tj-actions and Shai-Hulud as worked lessons. SLSA reference title corrected. |
| [§6](06-identity-and-credentials.md) Identity | Token exchange, resource indicators and rich authorization requests for auditable delegation. Issuer scope as part of the credential (Storm-0558). Certificate-lifetime trend chart. |
| [§7](07-attestation.md) Attestation | Evidence-binding sequence figure. Arm CCA added. |
| [§8](08-runtime-and-data.md) Runtime | Operation budgets as a control. Read-only Docker socket mounts called out as unsafe. |
| [§9](09-ai-agents.md) AI agents | New section. Agents as workloads with untrusted control flow; seven rules; MCP authorization. |
| [§10](10-internet-infrastructure.md) [§11](11-hardware.md) [§12](12-cryptography.md) | Hybrid post-quantum key exchange in transit. Memory-interposer attacks from 2024–2026 and GPU attestation binding. New section on cryptographic agility and post-quantum migration. |
| [§14](14-authorization-contract.md) [§15](15-revocation.md) | Release intent gains log inclusion, sender binding, nested expiry and budgets. The decision predicate is written out term by term. The revocation bound’s expiry horizon is split by validation model; nested expiry removes permit lifetime from the worst case; issuer clock error is shown to cancel. |
| [§16](16-failure-semantics.md) [§17](17-governance-and-recovery.md) | Failure-state figure. The control plane’s own blast radius (CrowdStrike). Quorum approval and logging of trust-root changes. |
| [§18](18-deployment-profiles.md) – [§23](23-requirement-traceability.md) | Profile, topology and adoption figures. Physical-access row in the profile table. Nine new acceptance tests. Metrics tied to the exposure model. Requirements R-13 to R-17. |
| References | All 40 original references kept and re-checked; exact dates added for NIST SP 800-63B-4 (July 2025) and the CISA/NSA memory-safety sheet (24 June 2025). New references cover incidents, transparency, delegation, post-quantum standards, TEE attacks and AI-agent security, all checked against primary sources on 24 October 2026. |
