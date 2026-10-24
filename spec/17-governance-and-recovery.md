# Recover the authority to trust a system

## Separation of administrative powers

The release owner approves application intent.
The security authority approves evidence profiles and resource policy.
Operations staff carry out approved deployments.
Small teams may hold several roles, but high-impact root changes need an independent approval path, and separate signing keys express the roles even when people overlap.
Changes to trust roots require a quorum (for example two of three key holders) and are recorded in the same transparency log as releases.

Every authority has a documented scope and a revocation procedure.
Root and recovery keys stay outside ordinary production workloads.
Hardware-backed keys help where supported, but the operations they allow still need authorization.
Management-plane access is restricted, and emergency use is recorded in an independent audit destination.

## Recoverable evidence and state

CISA recommends protected backups and tested restoration as ransomware preparation.
The fabric keeps independently accessible backups of essential application data and of the configuration needed to rebuild trust services.
A backup that production administrators can erase with the same credential does not meet the independence requirement. [@cisa-ransom]

Object-lock mechanisms preserve retained versions, and S3 documents distinct governance and compliance retention modes.
Retaining bytes does not show that the data is clean, that every dependency was backed up, or that the decryption key still exists.
Protect retention settings and key lifecycle independently. [@s3-lock]

## Recovery sequence

Establish a clean recovery control plane with reviewed trust anchors.
Suspend the compromised issuing authority and preserve incident evidence.
Reassess the approved release set, including the builder and the signer.
Deploy a clean instance and reissue only the permissions it needs.
Restore a selected data checkpoint after checking application consistency and signs of contamination.

Rejoining production requires the same admission checks as a new instance.
A recovery deployment is never exempt from policy because the service is urgent.
A separately approved emergency profile may reduce functionality while keeping the minimum containment boundary.

> [!NOTE]
> **Restore test as a security test.**
> An acceptance exercise rebuilds from a state in which the production identity service and the ordinary administrator account are unavailable.
> It verifies application correctness and key availability, not merely that files came back.

## Operational ownership

Assign an owner to each evidence profile and each trust root.
Record supported hardware and software versions.
Run an advisory-response process for changes to that trusted base.
Publish the maximum accepted age of verification collateral and the exception process for a failed patch rollout.
