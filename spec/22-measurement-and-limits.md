# Measure bounded harm and operational correctness

## Metrics with security meaning

Report each term of the exposure model, so a reader can see where harm is still unbounded.

| Term | Metrics |
|---|---|
| Reach | Sensitive operations with complete enforcement coverage; open bypasses; the most data reachable under one workload’s authorized role |
| Rate and budget | Budgets set against observed peaks; budget denials; time to drain a store at the permitted rate |
| Window | Time to detect and time to decide, reported separately from $`T_{\text{stop}}`$; the slowest tested covered path and the tail of the distribution |
| Correctness | False-denial rate; expiry enforcement during outages; recovery-point loss; time to restore trusted service |

A count of installed products does not establish any of these quantities.

## Known assurance gaps

Attestation coverage is bounded by the evidence collected.
The Keylime case study shows how hard it is to evaluate runtime integrity in changing systems.
Heterogeneous TEEs and runtime stacks need explicit interpretation of measurements; a common token format does not make every vendor’s evidence equivalent. [@eat; @keylime-dsn]

An authorized malicious release can operate inside its permitted scope.
Colluding administrators or compromised roots may authorize harmful policy.
A confidential guest can contain vulnerable code, and physical attacks remain outside current TEE models.
A model inside an agent can be steered within whatever its delegation allows.
Network partitions make immediate, globally consistent revocation impossible without cutting off isolated paths; the proposal resolves that trade-off with finite local deadlines for sensitive operations.

## Research and engineering agenda

- **Workload binding.** Independently reviewable tests connecting protected instance keys to the exact executable and launch policy, including late-loaded code and state migration.
- **Revocation semantics.** Validate maximum session duration across proxies and databases.
  Implement nested expiry in common token formats and gateways.
  Explore epoch recovery after state loss without allowing rollback or permanent denial.
- **Policy correctness.** Test whether generated least-privilege policies keep intended business operations while excluding prohibited ones.
  Policies produced by AI require deterministic validation and accountable approval.
- **Agent containment.** Measure how much useful work provenance-tracking gateways allow on real tasks, and publish the policies that make the trade-off acceptable.
- **Cryptographic migration.** Exercise post-quantum and hybrid rotation of release, firmware and transparency-log keys end to end.
- **Interoperability.** Publish a minimal profile specification with reference test vectors.
  Each implementation discloses unsupported claims and any downgrade behaviour.
