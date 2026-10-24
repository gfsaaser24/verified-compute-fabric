# The specification

This folder is the source of truth for Verified Compute Fabric.
Citations such as `[@slsa]` point to keys in [`references/references.yaml`](../references/references.yaml).

## Front matter

- [About this paper](00-about.md): What this paper is, and what it is not
- [Executive brief](00-brief.md): Limit what a breach can do

## Part I · The problem and the model

1. [Why breaches spread: the exposure model](01-why-breaches-spread.md)
2. [Scope, evidence and threat model](02-scope-and-threat-model.md)

## Part II · Architecture

3. [Control planes and trust boundaries](03-control-planes.md)
4. [The evidence-bound lifecycle](04-lifecycle.md)

## Part III · Mechanisms

5. [Software supply chain and transparency](05-supply-chain.md)
6. [Identity, credentials and delegation](06-identity-and-credentials.md)
7. [Attestation and launch binding](07-attestation.md)
8. [Runtime, data and operation budgets](08-runtime-and-data.md)
9. [AI agents as workloads](09-ai-agents.md)
10. [Internet infrastructure](10-internet-infrastructure.md)
11. [Hardware and the protected boundary](11-hardware.md)
12. [Cryptographic agility and post-quantum migration](12-cryptography.md)

## Part IV · Contracts

13. [Interoperability conventions](13-interoperability.md)
14. [The authorization contract](14-authorization-contract.md)
15. [Revocation and nested expiry](15-revocation.md)
16. [Failure semantics](16-failure-semantics.md)
17. [Governance and recovery](17-governance-and-recovery.md)

## Part V · Operations

18. [Deployment profiles](18-deployment-profiles.md)
19. [A Docker and Coolify adoption path](19-docker-adoption-path.md)
20. [Adoption gates and economics](20-adoption-gates.md)
21. [Validation: acceptance tests](21-acceptance-tests.md)
22. [Measurement, limitations and research agenda](22-measurement-and-limits.md)
23. [Requirement traceability](23-requirement-traceability.md)
24. [Conclusion](24-conclusion.md)

## Appendices

- [Appendix A: Terms used in this paper](A-glossary.md)
- [Appendix B: Changes from version 1.0](B-changes-from-v1.md)
