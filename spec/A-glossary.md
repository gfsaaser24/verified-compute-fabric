# Terms used in this paper

- **Attestation**: A process in which an attester produces signed evidence about its state and a verifier appraises it against reference values.
- **Budget**: A hard ceiling on what a lease may do, such as rows exported or calls made, enforced by the gateway regardless of how often the lease is renewed.
- **Confidential guest**: A virtual machine or container whose memory is protected from its host by processor features such as SEV-SNP, TDX or Arm CCA, under the vendor’s threat model.
- **Covered path**: A route by which an operation can reach a resource and on which enforcement is proven to apply.
  Any other route is a bypass.
- **Delegation**: Authority to act for a specific user or tenant, carried as a token that names both the acting workload and the party it acts for.
- **Enforcement point**: The component that evaluates each request and allows or denies it: a gateway, a database policy, a guest supervisor or a network control.
- **Epoch**: A signed, monotonically increasing policy version.
  Enforcement points persist the highest epoch they have accepted and reject anything older.
- **Evidence profile**: A named, versioned statement of what evidence is collected, what it covers and how it is appraised.
- **Expiry horizon**: How long valid authority can still exist on a path if a revocation message never arrives.
- **Exposure**: The harm a compromised authority can do before it stops: bounded by reach, rate, budget and window.
- **Host-adversarial profile**: A deployment profile that treats the host operator as hostile and relies on a protected guest and off-host verification.
- **Issuer lease**: The limited time for which an issuer may mint permits.
  An issuer cut off from revocation messages stops when its lease ends.
- **Lease (permit)**: A short-lived grant that names a resource, an action, a tenant, an audience, an expiry, a budget and a policy epoch.
- **Managed-host profile**: A deployment profile that trusts the host administrator and kernel and contains application compromise.
- **Nested expiry**: The rule that a permit is valid only while the authority that issued it is valid, checked at the time of use.
- **Prompt injection**: Instructions hidden in content an AI model reads, which steer the model toward actions its user did not request.
- **Provenance label**: Metadata that travels with a value and records where it came from, so policy can treat untrusted values differently.
- **Reference value**: An approved measurement, such as a digest of firmware or code, against which evidence is compared.
- **Relying party**: The component that uses an appraisal result to make a decision, such as releasing a credential.
- **Sender-constrained token**: A token bound to a key (through mutual TLS or DPoP) so that only the holder of that key can use it.
- **Transparency log**: An append-only, publicly auditable log.
  Inclusion proofs show an entry exists; monitors watch for entries nobody expected.
- **Trusted computing base**: Everything whose failure could break a stated security property: hardware, firmware, kernels, issuers, signers, verifiers and recovery operators.
