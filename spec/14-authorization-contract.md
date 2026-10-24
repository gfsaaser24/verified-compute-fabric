# Separate release intent from runtime permission

The example defines an illustrative release-intent object.
It is a schema specific to this proposal, with no out-of-the-box runtime support.
Placeholder digests are intentionally invalid.
A real implementation needs schema validation and a signed envelope with authorized issuers.

```yaml
type: fabric.example/release-intent
schema_version: "0.2"
subject: "spiffe://example.org/prod/tenant-a/exporter"
release:
  manifest_digest: "sha256:<64-hex-digits>"
  image_digest: "sha256:<64-hex-digits>"
  transparency_log_inclusion_required: true
execution:
  profile: "managed-host-v1"
  launch_policy_digest: "sha256:<64-hex-digits>"
  evidence_max_age_seconds: 300
access:
  audience: "customer-export-gateway"
  resource: "tenant-a/customer-export"
  actions: ["create-approved-export"]
  delegation_required: true
  sender_binding: "dpop"
  lease_max_seconds: 60
  nested_expiry: true              # a permit may not outlive its issuer's lease
  session_deadline_required: true
budgets:
  rows_per_export: 50000
  exports_per_hour: 4
failure:
  new_privileged_access: "deny"
  expired_lease: "terminate"
policy_epoch: 42
```

A release authority signs the intent.
A verifier independently returns a signed appraisal result bound to a fresh challenge and the authenticated instance key.
An authorization service then issues a separate permit that names the instance, the applicable policy, the expiry and the budget.
Each object has its own issuer role, and the application cannot create its own attestation result.

## The decision predicate

An enforcement point allows an operation only when every term holds:

```math
\begin{aligned}
\mathrm{Allow}(op) \iff\ & \mathrm{Auth}(c) \,\land\, \mathrm{Fresh}(ev, \pi) \,\land\, \mathrm{Bound}(rel, inst) \\
\land\ & \mathrm{Permits}(\ell, op) \,\land\, \mathrm{Current}(\ell, e) \,\land\, \mathrm{Within}(\ell, op) \\
\land\ & \big(\lnot\,\mathrm{SenderBound}(\pi) \,\lor\, \mathrm{PoP}(c, \ell)\big)
\end{aligned}
```

| $`\mathrm{Auth}(c)`$ | The caller *c* is authenticated as the instance named in the lease. |
|---|---|
| $`\mathrm{Fresh}(ev, \pi)`$ | The evidence the lease relied on is still within the maximum age of execution profile *π*. |
| $`\mathrm{Bound}(rel, inst)`$ | The approved release and launch policy match the running instance. |
| $`\mathrm{Permits}(\ell, op)`$ | Lease *ℓ* names this resource, action and tenant, with a valid user delegation where required. |
| $`\mathrm{Current}(\ell, e)`$ | The lease and its issuer’s authority are unexpired on the enforcement point’s clock, and the lease epoch is at least the epoch *e* this point has accepted. |
| $`\mathrm{Within}(\ell, op)`$ | The operation fits the lease’s remaining budget. |
| $`\mathrm{PoP}(c, \ell)`$ | Where the profile requires sender binding, the caller proves possession of the key bound into the lease. |

The managed-host example relies on the host for instance measurement and launch enforcement.
Choosing a confidential-guest profile requires an attested guest supervisor and a verified binding to its enforced launch policy.
Changing the profile string alone does not create hardware-backed protection.
