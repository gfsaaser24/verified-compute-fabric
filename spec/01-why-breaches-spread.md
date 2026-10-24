# Breaches spread through authority, not through the first bug

Public incident reviews tell a consistent story.
The way in varies: a stolen password, a poisoned build, a forged token.
What turns it into a crisis is the authority waiting on the other side.

The table lists seven well-documented incidents and the boundary each one shows to be missing or too weak.
None of this is a claim that the architecture in this paper would have prevented the incident.
Each case shows which limit mattered.

| Incident | What happened | Boundary it shows |
|---|---|---|
| **SolarWinds**, 2020 | An implant on the vendor’s build server swapped a source file during compilation, so the vendor’s own signed updates carried a backdoor. [@sunspot] | Isolated, verifiable builds with provenance ([§5](05-supply-chain.md)) |
| **Storm-0558**, 2023 | Tokens signed with a 2016 consumer signing key, meant to be retired in 2021, were accepted for enterprise mailboxes because of a validation flaw. Twenty-two organizations and over 500 individuals were affected. [@csrb-storm] | Scoped issuers whose authority expires; resources reject out-of-scope issuers ([§6](06-identity-and-credentials.md)) |
| **Snowflake customers**, 2024 | Credentials stolen by infostealer malware, some unrotated for up to four years, were used against accounts with no second factor and no network allow-list. About 165 organizations were notified. [@unc5537] | Short-lived, sender-bound credentials; phishing-resistant login ([§6](06-identity-and-credentials.md)) |
| **xz-utils**, 2024 | A maintainer identity built up over more than two years published release tarballs containing a build trigger that was absent from the public repository. It activated a payload hidden in committed test files. It was disclosed before wide deployment. [@xz] | Build from reviewed source; runtime limits still apply to signed software ([§5](05-supply-chain.md), [§8](08-runtime-and-data.md)) |
| **CrowdStrike**, 2024 | A faulty security-content update went to Windows hosts all at once and was pulled 78 minutes later. A template expected 21 input fields and received 20. About 8.5 million devices crashed. [@crowdstrike-rca; @ms-crowdstrike] | The control plane needs its own blast radius: staged rollout and canaries ([§16](16-failure-semantics.md)) |
| **tj-actions**, 2025 | Version tags of a GitHub Action used by over 23,000 repositories were moved to a malicious commit that printed CI secrets into build logs. [@tj-actions] | Pin by digest; short-lived, scoped build secrets ([§5](05-supply-chain.md)) |
| **Shai-Hulud**, 2025 | A self-replicating npm worm stole developer and cloud tokens and used them to publish infected versions of more than 500 packages. [@shai-hulud] | Publishing without long-lived tokens ([§5](05-supply-chain.md)) |

## The exposure model

Treat compromise as a given and ask how much harm it can do before it is stopped.
For an attacker who holds authority $`a`$, the harm is bounded by

```math
E(a) \;\le \sum_{r \,\in\, \mathrm{Reach}(a)} \min\!\big(B_r,\ \rho_r \cdot W\big), \qquad W = T_{\text{detect}} + T_{\text{decide}} + T_{\text{stop}}
```

$`\mathrm{Reach}(a)`$ is the set of resources and actions the authority can exercise.
$`\rho_r`$ is the fastest rate at which the attacker can act on resource $`r`$.
$`B_r`$ is a hard budget on $`r`$, such as a cap on rows exported per lease.
$`W`$ is the window from compromise until the authority stops working.

Each design rule shrinks one term.
Per-operation grants and tenant binding shrink Reach.
Rate limits and operation budgets cap $`\rho`$ and $`B`$.
Short leases, nested expiry and push revocation shrink $`T_{\text{stop}}`$.
Detection time is outside this paper’s controls, but it sits inside $`W`$, so it must be measured.

![Exposure under two credential designs and two kinds of attacker](../figures/exposure.svg)

**Figure 1.** Exposure under two credential designs and two kinds of attacker.
The shaded area is exposure in tenant-hours of reachable data. *Illustrative model:* 50 tenants; compromise detected at 48 h in every panel; a standing key takes 6 h to rotate; a lease revocation takes effect within 10 s of a 30-minute decision; under leases a compromised workload can act only for tenants whose users are active, one to six at a time.

**Two attackers, two levers.** The model separates two cases that are often blurred.
An attacker who *steals a credential* uses it from outside.
Sender binding makes the stolen token useless without its key, and a short lease caps what is left.
The detection time does not matter.
An attacker who *runs code inside the workload* renews leases as the workload.
There the window is set by detection, so Reach and budgets do the work.
This is why the architecture never relies on short lifetimes alone.

> [!NOTE]
> **Limits of the model.**
> The bound counts what authority allows.
> It does not count harm from data already read, which cannot be recalled, from denial of service, or from paths the enforcement layer does not cover.
> An uncovered path has unbounded reach.
