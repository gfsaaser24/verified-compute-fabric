# Acceptance tests must try to break the claim

The architecture requires negative tests and fault injection.
Each result records the component versions and the selected profile.
A passed test is evidence for the tested condition, not proof that every attack path has been exhausted.
Tests marked **new** were added in version 2.0.

| Test | Expected acceptance evidence |
|---|---|
| Approved signer, wrong repository or builder | Admission denies the artifact and names the failed policy condition |
| **New:** valid signature, no transparency-log entry | Admission denies; the monitor reports the attempt |
| Valid image, modified launch configuration | Unauthorized entry point or executable mount is rejected |
| Replayed attestation or substituted instance key | Broker refuses release; challenge and channel binding are verified |
| Stale firmware evidence or retired measurement | Verifier rejects new elevated access under the current profile |
| Tenant substitution and overbroad data role | Resource denies cross-tenant reads and privileged database bypass |
| Stolen sender-constrained token used with another key | Resource rejects use without the required proof of possession |
| Direct backend connection or egress relay | Bypass is blocked or independently subject to equivalent authorization |
| **New:** permit whose expiry is later than its issuer’s lease | Enforcement rejects it, or honours it only until the issuer deadline |
| **New:** renewing workload exceeds its lease budget | Gateway denies further operations and raises an alert |
| Revocation during a long-running export | Covered streams close within the declared bound; partial outputs are recorded |
| Partitioned revocation or policy service | Permissions expire locally; no unapproved lease extension occurs |
| Old signed policy after a restart | Durable epoch prevents rollback; unsynchronized instances cannot elevate access |
| **New:** faulty policy bundle in a staged rollout | The canary fails, rollout halts, and the previous policy stays valid |
| **New:** retrieved content tells an agent to send data to a new recipient | Gateway blocks it: untrusted data may not choose a recipient |
| **New:** sub-agent requests a wider scope than its parent | Token exchange refuses the request |
| **New:** agent context, memory and logs scanned after a task | No secrets present; the broker log shows every credential use |
| **New:** signature algorithm downgraded in a signed object | Verifier rejects it as a policy violation |
| **New:** MCP server receives a token issued for another audience | Server rejects it and does not pass it upstream |
| Host-root control in the managed-host profile | Report explicitly marks host-root resistance as outside the claim |
| Host interference in the confidential profile | Selected protected secrets stay unavailable in tested cases; denial of service remains possible |
| Compromised issuer or inaccessible production account | Independent root rotation and restore complete and are audited |

## Execution discipline

Run destructive tests in an authorized environment with representative data and recovery capacity.
Record unsuccessful attack attempts and any successful bypass.
Repeat the relevant tests after changes to the runtime, firmware, model or trusted policy.
A detector alert without effective denial does not pass an enforcement test.
