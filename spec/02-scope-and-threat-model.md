# Research basis and threat model

This paper is an architectural synthesis of public standards, implementation documentation and research reviewed on 24 October 2026.
It presents no prototype measurements.
Product documentation establishes supported mechanisms and their stated assumptions.
It does not validate a complete deployment.
Cited research is used within the scope of the system it studied.
MUST and SHOULD express requirements of this reference architecture; they do not amend the cited standards or imply endorsement by their authors.

## Attacker capabilities

| Threat | Required boundary | Residual exposure |
|---|---|---|
| Malicious package or application process | Independent identity issuance and resource enforcement | Permitted business operations and data already reachable |
| Compromised build job or developer credential | Isolated builders; release approval outside that job | Malicious approved source or a compromised release authority |
| Compromised maintainer or dependency, validly signed | Source review; build from source; runtime limits | Logic that stays inside its permitted scope |
| Stolen workload token | Short leases; audience and sender binding | A compromised caller that also holds the binding key |
| Prompt-injected AI agent | Deterministic tool gateway; task-scoped delegation; no secrets in model context | Actions the user’s delegation allows |
| Stolen issuer or signing key | Scoped issuers; short issuer leases; transparency logs | Tokens minted within the key’s remaining scope and lifetime |
| Compromised host or provider administrator | Host-adversarial profile with protected guest enforcement | Hardware or firmware failure, guest compromise, denial of service |
| Physical access to memory buses | Declared out of scope by current TEE vendors; physical security | Guest secrets and attestation keys on affected platforms |
| Compromised fabric authority | Scoped issuers; independent recovery and key rotation | Privileges inside that authority’s scope |
| Network partition or destructive attack | Explicit expiry behaviour; independently recoverable state | Service interruption and transactions already committed |

## Assumptions every deployment declares

Every deployment records its trusted computing base: hardware and firmware, the software that can change policy, identity issuers, release signers, attestation verifiers and recovery operators.
It also names the attack classes it excludes, such as physical attacks or side channels outside the selected hardware model.

Administrative independence needs different compromise paths.
Two services on one root-controlled host do not protect each other from that host.
Separate accounts can still share one recovery credential.
A review traces who controls signing keys, network bypasses and backup deletion.
