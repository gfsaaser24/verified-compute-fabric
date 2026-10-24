# Approve the release that actually runs

## Protected source and isolated builds

Use SSDF-aligned development practices and target SLSA v1.2 Build Level 3 where the build platform supports it.
SLSA’s build requirements include protected provenance and build isolation.
A project assesses its platform and its own responsibilities before claiming a level; a signature alone does not establish SLSA conformance. [@ssdf; @slsa]

Build workers are separate from production.
Ephemeral workers get only the inputs and publishing authority their job needs.
They never hold production deployment keys or the power to approve their own output.
Untrusted build steps never get a management host’s Docker socket.
SolarWinds is the reference case: an implant on the build server replaced a source file during compilation, and the vendor’s own signed builds carried the result. [@sunspot]

Publishing credentials are build secrets too.
The Shai-Hulud worm spread by stealing developers’ long-lived package-registry tokens and publishing with them.
Trusted publishing, in which a CI job proves its identity with a short-lived OIDC token instead of a stored secret, had been generally available for npm since July 2025, six weeks before the worm appeared. [@shai-hulud; @npm-trusted]

## Verifiable release envelope

The release envelope MUST include the artifact digest, the source revision and the builder identity.
It also lists the dependency inventory and the security-relevant launch configuration. in-toto can express attestations about each supply-chain step.
Cosign can check the expected certificate identity and issuer while it verifies an artifact signature; the release policy pins those expectations to the intended repository and workflow. [@in-toto; @cosign]

Use SPDX for the software bill of materials.
An inventory supports vulnerability response and dependency identification.
It is not evidence that every component is benign.
A vulnerability exception has an owner and an expiry condition. [@spdx]

## Record every approval in a transparency log

A signature proves who approved a release.
It does not show anyone else that the approval happened.
Record each approval in an append-only transparency log, require proof of inclusion at admission, and let independent monitors watch the log for entries the release owner did not expect.
Sigstore’s Rekor provides this for signatures, Certificate Transparency is the model on the web, and the IETF SCITT architecture generalizes it to supply-chain statements.
A stolen signing key then leaves public evidence of its own misuse instead of buying silent trust. [@rekor; @ct2; @scitt]

## Admission binding and update security

The admission controller resolves immutable digests and checks that the approved configuration is the one supplied to the runtime.
It rejects unauthorized mounts, changed entry points and unreviewed executable downloads.
Reproducible builds add a consistency check.
Matching output does not make the source safe.

Mutable references are an attack surface.
In March 2025 the version tags of the tj-actions/changed-files GitHub Action were moved to a malicious commit that printed CI secrets into build logs.
Pinning every action and image to a full digest, and giving build jobs only short-lived, narrowly scoped secrets, limits that class of attack. [@tj-actions]

Use TUF-style signed update metadata with separated roles, threshold signing where appropriate and explicit expiry.
Version checks stop an attacker from restoring a release that was valid once and has since been revoked.
An emergency rollback is a newly authorized decision with an audit record, not an exemption from rollback protection. [@tuf]

> [!WARNING]
> **Malicious but validly signed.**
> The xz-utils backdoor came from a co-maintainer identity that had spent more than two years earning trust.
> The build trigger was only in the release tarballs; the payload was hidden in test files committed to the repository.
> Every signature check would have passed.
> Build from reviewed source where possible, compare release archives with the repository, and keep runtime limits and resource authorization mandatory after every supply-chain check succeeds. [@xz]

## Engineering convention

Store the release manifest and its signatures apart from the application image.
Production pulls the image by digest and verifies the release authorization independently.
Release signing keys are never available to application containers or ordinary build jobs.
