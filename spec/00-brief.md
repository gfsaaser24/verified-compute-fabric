# Limit what a breach can do

Large breaches rarely need a large first foothold.
They need authority that is broad, long-lived and controlled by the thing that was compromised.

Think of a service key that can read every tenant, a token that works from any network for months, or an admin path nobody watches.
The first mistake is small.
The authority behind it is not.
Public reviews of SolarWinds, Storm-0558 and the Snowflake intrusions show the same pattern.
The way in differed every time.
The damage came from what the first foothold could reach.

Verified Compute Fabric binds every protected operation to authenticated evidence about the running workload and to a current, resource-specific policy.
It issues authority as short leases.
Components the workload does not control enforce them.
Authorities kept out of an attacker’s reach revoke or rebuild them.

The goal is a damage bound you can compute and test: what a compromised workload can reach, how fast it can act and for how long.
On the illustrative numbers in [Figure 1](../figures/exposure.svg), exposure falls from about 2,700 tenant-hours to about 70 when the attacker runs inside the workload.
For a stolen token it falls to almost nothing.

### Five design rules

1. **Prove what runs.** Approve exact code and launch configuration, record the approval in a public log, and bind evidence of that release to the running instance.
2. **Grant per operation.** Authority names a resource, an action, a tenant and an audience.
   Identity alone grants nothing.
3. **Make every grant expire.** Leases are short.
   Renewal needs fresh evidence.
   No grant outlives the authority that issued it.
4. **Enforce from outside.** Decisions are checked where the workload cannot reach: a gateway, a database policy, a protected-guest supervisor, an off-host broker.
5. **Keep recovery out of reach.** Restoring trust must work when production identity, policy and administrator accounts are gone.

### Deployment decision

The **managed-host profile** trusts the host administrator and kernel, and contains application compromise with external identity and resource-level authorization.
The **host-adversarial profile** adds a supported confidential execution environment, an attested guest supervisor and an independent verifier.
Its guarantees depend on the hardware vendor’s security model and the protected guest’s code.
Neither profile makes authorized software harmless.
Neither recalls data already disclosed.

### Where to start

Pick one sensitive operation, such as a tenant data export.
Put it behind an independent gateway with a 60-second lease, sender-bound tokens, a hard export budget and a tested revocation path.
Build releases on a separate host, deploy only exact digests, and measure the result.
Expand only when the operation passes its gate ([§20](20-adoption-gates.md)).
Record every control you have not yet built as an assurance gap.
