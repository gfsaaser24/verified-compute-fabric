# Contain execution and constrain business actions

## Application isolation

The managed-host baseline runs services without unneeded privilege: non-root execution, restricted system calls, a mandatory-access-control profile, no writable executable mounts, resource ceilings and no unused capabilities.
Docker treats access to its daemon as an administrative capability.
Giving an application that access is outside this baseline. [@docker-sec]

Mounting the Docker socket read-only does not help.
The flag protects the socket file, not the API behind it: a process that can connect can still change containers and the host.
A service that needs container metadata, such as a reverse proxy doing service discovery, reaches the daemon through a filtering proxy that exposes only the read endpoints it needs. [@owasp-docker]

For an extra isolation boundary, evaluate gVisor or Firecracker against the workload’s system-call and operational needs. gVisor runs a sandbox with its own security model.
Firecracker provides microVMs.
Neither protects a workload from a malicious host administrator. [@gvisor; @firecracker]

Host-based eBPF policy and telemetry help within the managed-host profile, and Cilium documents the trust boundaries of its deployment.
A compromised host kernel cannot be trusted to enforce rules against itself.
The host-adversarial profile therefore places essential decisions off-host or inside a protected guest. [@cilium]

## Application and database authorization

A gateway validates the requested action and its tenant context.
Use narrow database roles and, where appropriate, row-level security.
PostgreSQL superusers and roles with BYPASSRLS bypass row policies, and so do table owners unless FORCE ROW LEVEL SECURITY is set.
Application connections never inherit those privileges. [@pg-rls]

A tenant identifier comes from authenticated delegation and survives every hop without substitution by the caller.
A session variable that an attacker can set is not a tenant boundary.
Background jobs explicitly own the records they process.
Export and deletion need policy that reflects their impact.

## Operation budgets

Budgets turn the exposure model’s $`B_r`$ into a control.
Each lease carries hard ceilings that the gateway enforces: rows per export, exports per hour, bytes per response, distinct tenants per hour.
A budget stops a compromised workload that renews legitimately from draining a store before anyone notices.
Set budgets from observed use plus headroom, and alert as use approaches the limit, not only when it is hit.

## New software conventions

Use memory-safe languages for new security-sensitive components where the ecosystem allows.
CISA and NSA recommend memory-safe development to remove a large class of vulnerabilities.
Unsafe interfaces and foreign-function boundaries still need review, and memory safety does not fix authorization flaws or malicious logic. [@cisa-msl]

> [!NOTE]
> **Containment limit.**
> A compromised application may misuse data it is allowed to read.
> Budgets and resource-side controls bound that exposure only as far as the business operation itself can be constrained.
