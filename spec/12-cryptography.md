# Migrate first where trust lives longest

Every boundary in this paper rests on signatures and key exchange.
NIST published its first post-quantum standards in August 2024: ML-KEM for key establishment, and ML-DSA and SLH-DSA for signatures.
Its draft transition plan proposes deprecating today’s quantum-vulnerable algorithms at the 112-bit security level after 2030 and disallowing them after 2035. [@fips203; @fips204; @fips205; @ir8547]

Finite authority helps here.
An object that lives for minutes can switch algorithm at its next renewal.
The work concentrates on what lives for years.

| Object | Lifetime | Migration priority and approach |
|---|---|---|
| Leases, permits, workload certificates | minutes | **Low.** Switch at renewal once issuers and verifiers support the new algorithm. |
| Key exchange for data that must stay secret | as long as the data | **High now.** Recorded traffic can be decrypted later. Use hybrid ML-KEM key exchange ([§10](10-internet-infrastructure.md)). |
| Release and update signing keys | years | **High.** Plan ML-DSA or SLH-DSA, or hybrid signatures, with TUF-style key rotation. |
| Firmware signing and hardware roots | device lifetime | **Highest.** Stateful hash-based signatures (LMS, XMSS) are already specified, and NSA’s CNSA 2.0 sets 2030 for exclusive use of quantum-resistant firmware signing in national security systems. [@sp800-208; @cnsa2] |
| Transparency logs, archived evidence | indefinite | **High.** Use algorithm-agile log formats, or plan to re-sign checkpoints. |
| Backups encrypted for long retention | years | **High.** Re-wrap data keys under post-quantum or hybrid schemes. |

## Conventions

Every signed or encrypted object names its algorithm explicitly.
Verifiers never infer it from attacker-controlled data.
Policy lists accepted algorithms per object type, so a downgrade is a policy violation, not a parsing choice.
Inventory where each algorithm is used before choosing a migration order.
Test the rotation of every trust anchor now: the hard part of a migration is rotation, not the new mathematics.

> [!NOTE]
> **Do not wait for certainty.**
> No one knows when a quantum computer will be able to break today’s public-key cryptography.
> Long-lived roots must move before that happens, and the path to move them must already have been tested.
