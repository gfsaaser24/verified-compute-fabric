# Make the protected boundary explicit

Hardware is chosen for an adversary model.
A processor feature or a security chip does not make all software on the machine safe.
The deployment states what is protected and who can change that protection.

| Mechanism | Intended role | Boundary that must be stated |
|---|---|---|
| Secure or measured boot, TPM | Verify boot policy and authenticate selected measurements | Does not hide guest memory from a host operator |
| HSM or hardware-backed key | Protect key material against extraction | A compromised but authorized caller can still use the key |
| SEV-SNP, TDX, Arm CCA | Protect confidential guests under a vendor threat model | Guest code, firmware state and attestation verification stay dependencies; physical attacks are excluded |
| Confidential GPU | Extend the protected boundary to accelerator memory | Driver, firmware and the GPU attestation chain join the trusted base; GPU evidence must be bound to its guest |
| Restricted DPU or SmartNIC | Enforce network policy outside host control | Only traffic on the governed path is covered |
| CHERI capabilities | Enforce capability bounds and compartments in hardware | Needs compatible software and a defined profile |

SEV-SNP, TDX and Arm CCA are candidates for the host-adversarial profile, subject to supported platforms and verified launch semantics.
The host can still deny scheduling or connectivity.
Confidential execution does not make vulnerable guest code trustworthy. [@snp; @tdx; @arm-cca]

## Physical access is outside the current threat models

Research in 2024–2026 showed what that exclusion means in practice.
BadRAM tampered with a memory module’s configuration chip to create memory aliases and broke SEV-SNP attestation; AMD shipped a firmware check.
Battering RAM used an interposer costing under \$50 on DDR4 buses against Intel SGX and SEV-SNP, and WireTap passively extracted an SGX attestation key.
TEE.fail moved to DDR5: it extracted the Intel key needed to forge TDX attestation quotes and recovered signing keys from inside SEV-SNP guests.
In September 2026, DDRop showed that dropping DDR5 writes breaks the integrity of SGX, TDX and SEV-SNP, because none of them checks memory freshness.
For Battering RAM, WireTap and TEE.fail, the vendors pointed to threat models that exclude physical access. [@badram; @battering-ram; @amd-sb-3024; @wiretap; @teefail; @ddrop]

Two lessons follow.
A host-adversarial claim must state who can physically touch the machine.
And evidence must be bound to its receiver: TEE.fail paired a forged CPU attestation with a genuine GPU attestation relayed from another machine, because the GPU report was not tied to a specific confidential guest. [@teefail; @nvidia-cc]

## Network and capability hardware

NVIDIA documents a restricted BlueField mode that separates host privileges from DPU administration.
The mode must be enabled and its management credentials isolated; deploying the card does not establish that separation.
The DPU’s firmware and management environment join the trusted base. [@bluefield] CHERI provides hardware capabilities for memory access and compartments.
It is an optional evaluation track here, and the baseline does not depend on it. [@cheri]

## Firmware is an operational dependency

Keep authenticated firmware updates and independently usable recovery, consistent with NIST platform-firmware resiliency guidance.
Hardware-backed claims must follow security advisories.
AMD-SB-3019 describes a microcode signature-verification flaw affecting SEV-SNP protections, and its mitigations.
A verifier that ignores affected firmware accepts an obsolete assumption. [@sp800-193; @amd-sb-3019]
