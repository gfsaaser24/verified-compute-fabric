# Changelog

Changes to the Verified Compute Fabric concept.
Version rules are in [CONTRIBUTING.md](CONTRIBUTING.md#versions).

## 2.0 · 24 October 2026

Revised and extended edition. Section-by-section detail is in [Appendix B](spec/B-changes-from-v1.md).

- Added the exposure model: harm is bounded by reach, rate, budget and time window ([§1](spec/01-why-breaches-spread.md)).
- Added nested expiry: no permit outlives the authority that issued it, which takes permit lifetime out of the revocation worst case ([§15](spec/15-revocation.md)).
- Added AI agents as workloads ([§9](spec/09-ai-agents.md)), cryptographic agility and post-quantum migration ([§12](spec/12-cryptography.md)), transparency logging of release approvals ([§5](spec/05-supply-chain.md)) and operation budgets ([§8](spec/08-runtime-and-data.md)).
- Added seven verified incidents and the 2024–2026 memory-interposer attacks on confidential computing ([§11](spec/11-hardware.md)).
- Added 12 figures, nine acceptance tests and requirements R-13 to R-17.
- Corrected the SLSA reference title and made two reference dates exact. Kept and re-checked all 40 version 1.0 sources; added 45.
- Co-Authored with Human Assistance from: Gabe Fletcher.
- Moved into this repository as the source of truth: text in Markdown, sources in YAML.
- Licensed under the Apache License 2.0.

## 1.0 · 24 October 2026

First draft, written with ChatGPT. Kept unchanged in [`releases/v1.0/`](releases/v1.0/).
