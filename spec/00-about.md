# What this paper is, and what it is not

Co-Authored with Human Assistance from: **Gabe Fletcher**

Verified Compute Fabric is a reference architecture.
It takes security mechanisms that already exist — zero-trust access control, remote attestation, workload identity, supply-chain provenance, confidential computing — and defines the contract that makes them work together.
It then says how to test that contract.
The standards it cites support individual properties.
They do not establish the security of the integrated design described here.

“Verified” has one meaning in this paper: a named verifier has authenticated evidence and evaluated named claims against a versioned policy.
It is not a claim of formal verification, universal safety or certification.
The name does not designate an industry standard.

#### This paper provides

- Design rules and a reference composition of control planes.
- An exposure model that ties each control to the harm it limits.
- Analytical bounds on revocation, with every assumption stated.
- Requirements traceable to controls and to acceptance tests.
- An adoption path for a small Docker-based estate.

#### This paper does not claim

- An implementation, a benchmark or a certification.
- That authorized software is harmless.
- That data already disclosed can be recalled.
- Availability under resource exhaustion or denial of service.
- Protection from physical attacks on memory, unless declared.

Figures and numbers marked *illustrative* use stated parameters, not measurements.
Every real-world date, figure and incident was checked against a primary source on 24 October 2026.

## What changed in version 2.0

- **An exposure model.** Harm is bounded by reach, rate, budget and time window ([§1](01-why-breaches-spread.md)).
  Every control now maps to one of those terms.
- **A tighter revocation bound.** Delegated authority may not outlive the authority that issued it (“nested expiry”).
  This takes permit lifetime out of the worst case, so permits can be cached longer without weakening revocation ([§15](15-revocation.md)).
- **AI agents as workloads.** A new section treats agents as programs whose control flow can be steered by their inputs ([§9](09-ai-agents.md)).
- **Transparency and cryptographic agility.** Release approvals go into an append-only log ([§5](05-supply-chain.md)), and long-lived roots get a post-quantum migration plan ([§12](12-cryptography.md)).
- **Evidence from real incidents.** Seven public incidents anchor the boundaries ([§1](01-why-breaches-spread.md)).
  Recent memory-interposer attacks sharpen the hardware claims ([§11](11-hardware.md)).
- **Figures, tests and traceability.** 12 figures, nine new acceptance tests and five new requirements (R-13 to R-17).
  Cautions are stated once, here.
  [Appendix B](B-changes-from-v1.md) has the full change log.
