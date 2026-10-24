# Map every promise to an observable control

The identifiers below are proposed conformance requirements.
The cited foundations establish relevant mechanisms; the integrated guarantees need the acceptance evidence described here.
Requirements R-13 to R-17 are new in version 2.0.

| ID / requirement | Implementation evidence | Foundation |
|---|---|---|
| R-01 / Declare the trusted boundary | Profile names host assumptions and the complete trusted computing base | NIST zero trust; RATS [@nist-zta; @rats] |
| R-02 / Bind approval to execution | Approved artifact and launch configuration match the running instance | SLSA; in-toto; Cosign [@slsa; @in-toto; @cosign] |
| R-03 / Authenticate workload identity | Registration and instance mapping reject caller-controlled impersonation | SPIFFE / SPIRE [@spiffe; @spire] |
| R-04 / Bind evidence to the receiver | Fresh attestation and channel key are tied to the enforced launch state | RATS; Trustee; guest policy [@rats; @coco-attest; @coco-secure] |
| R-05 / Constrain each operation | Resource checks audience, tenant and action, with no ungoverned bypass | NIST cloud-native access; Envoy [@nist-zta-cn; @envoy-extauthz] |
| R-06 / Limit credential use | Lease scope and supported sender binding are enforced by the resource | OAuth mTLS; DPoP [@oauth-mtls; @dpop] |
| R-07 / Enforce policy freshness | Signed version, validity and durable epoch reject rollback | TUF; OPA [@tuf; @opa-bundles] |
| R-08 / Terminate revoked authority | All covered paths enforce the tested lease and session deadline | Proposed integration requirement |
| R-09 / State outage behaviour | Partition tests demonstrate explicit denial and recovery behaviour | Proposed integration requirement |
| R-10 / Protect and recover roots | Independent recovery restores service after loss of production authority | Firmware resilience; recovery guidance [@sp800-193; @cisa-ransom] |
| R-11 / Maintain the hardware profile | Firmware and collateral policy excludes unacceptable trusted-base states; physical-access exclusions are declared | Vendor attestation and advisories [@snp; @tdx; @amd-sb-3019] |
| R-12 / Publish limitations | Conformance report lists residual risks and unimplemented controls | Proposed governance requirement |
| R-13 / Log every approval | Admission requires proof of log inclusion; monitors watch for unexpected entries | Rekor; CT 2.0; SCITT [@rekor; @ct2; @scitt] |
| R-14 / No authority outlives its issuer | Enforcement checks issuer authority at use; permits beyond it are rejected | X.509 path validation [@rfc5280] |
| R-15 / Budget every sensitive lease | Gateway enforces per-lease ceilings and alerts as use approaches them | Proposed integration requirement |
| R-16 / Contain agent authority | No secrets in model context; task-scoped delegation; deterministic gateway; human confirmation for irreversible actions | OWASP LLM; CaMeL; MCP; token exchange [@owasp-llm; @camel; @mcp-auth; @token-exchange] |
| R-17 / Plan cryptographic migration | Algorithm inventory; explicit algorithm IDs; tested rotation plan for long-lived roots | FIPS 203–205; SP 800-208 [@fips203; @fips204; @fips205; @sp800-208] |

A conformance report includes the release set, profile versions and test results.
It names the parties authorized to change each trust anchor.
Claims are limited to the tested configuration and the named threat model.
Certification or independent evaluation would need a separate process.
