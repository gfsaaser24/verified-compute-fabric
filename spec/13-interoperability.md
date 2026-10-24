# Specify what each claim means

A deployable fabric needs a versioned implementation profile.
The profile defines the supported evidence and authorization formats, including their failure behaviour.
Existing protocols stay unchanged.
Profile-specific metadata is clearly namespaced and validated; it never silently reinterprets a standard field.

| Convention | Proposed requirement |
|---|---|
| Identifiers and scope | Stable logical workload ID; separate instance ID; explicit environment and tenant; no wildcard federation by default |
| Artifact identity | Full content digest with an algorithm identifier; configuration and launch-policy digests where security-relevant; transparency-log entry reference |
| Evidence semantics | Named profile version, measured boundary, nonce, verification audience and reference-value policy version |
| Authorization | Resource and action, authenticated delegation, expiry, issuer authority expiry, budget, sender binding where supported, policy epoch |
| Signed objects | Explicit object type and issuer; approved algorithms and keys per object type; reject unknown critical fields and ambiguous encodings |
| Revocation events | Unique event ID, scoped subject, durable monotonic epoch, effective time and acknowledgement state |
| Audit records | Decision ID, subject and release binding, policy version, outcome and expiry; sensitive values excluded |

## Serialization and signature discipline

Use the serialization and signature rules of the selected envelope.
A custom JSON object may use a defined canonicalization profile, such as RFC 8785, where appropriate.
The implementation specifies exact bytes and field handling.
It never mixes incompatible canonicalization rules or infers the algorithm from attacker-controlled data. [@jcs]

## Policy distribution

OPA supports signed bundles and keeps its previous bundle when a new one fails verification.
Its documentation notes that some JWT claims, including `iat` and `iss`, are not interpreted by bundle verification itself.
A fabric therefore implements and tests its own freshness and issuer-selection rules.
Bundle signature verification is configured as mandatory, and a valid signature does not show that a bundle is the currently authorized policy. [@opa-bundles]

A signed, monotonic policy epoch and an explicit maximum validity establish when a cached decision expires.
Each enforcement point persists its accepted epoch and resynchronizes after losing state.
An administrative rollback is a newly authorized version with its reason recorded.

> [!NOTE]
> **No undefined trust score.**
> An attestation result exposes the claims that passed and the profile used to evaluate them.
> Authorization never relies on an unexplained aggregate score that hides an unmet requirement.
