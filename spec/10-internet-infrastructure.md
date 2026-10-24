# Defend transport without a global authority

## Routing integrity and source validation

Publish correct route-origin authorizations and deploy RPKI origin validation in stages.
NIST provides deployment guidance, and RFC 6811 defines the validation behaviour.
Origin validation does not authenticate the full BGP path.
A route with no applicable authorization is not the same as an invalid one. [@nist-rov; @rfc6811]

Deploy source-address validation consistent with BCP 38.
It stops spoofed traffic.
Botnets sending from real addresses remain, so attack absorption and service-level controls are still needed. [@bcp38]

## DDoS mitigation and service continuity

Use upstream mitigation sized for the threat model.
Validate the protected origin path and remove public bypasses to a shielded backend.
Critical services need tested DNS recovery and independent administrative access.
Check geographic separation against shared upstream and account dependencies.

DOTS defines an open signal channel for requesting DDoS mitigation.
A fabric can send authenticated mitigation requests through participating providers if each request covers only resources the requester controls, expires, and is validated by the recipient.
This paper does not propose a new universal blocking protocol. [@dots]

## Workload connectivity

Mutual authentication establishes peer identity.
Resource policy still decides access.
Restrict egress to required services and deny direct paths around the credential broker.
A destination allow-list must account for proxy relays and DNS changes.

The network control plane keeps authority over its enforcement devices when an application fails.
A compromised host may need an independent switch, a provider firewall or a restricted DPU policy to cut its traffic.
Traffic on ungoverned interfaces is outside that claim.

## Post-quantum key exchange in transit

Use hybrid post-quantum key exchange on TLS paths that carry data with a long confidentiality life.
Chrome has offered X25519MLKEM768 by default since late 2024, and Cloudflare reported in April 2026 that more than 65% of human traffic to its network was post-quantum encrypted.
This protects recorded traffic against later decryption.
It does not change authentication, which [§12](12-cryptography.md) covers. [@chrome-pq; @cf-pq]

> [!NOTE]
> **Availability boundary.**
> Identity checks cannot restore a saturated link.
> An operator that controls power or scheduling can still deny service to a confidential guest.
> Availability needs capacity planning and independently exercised recovery routes.

## Privacy convention

Federated decisions disclose only the claims a partner needs.
Raw device identifiers and internal measurements are not shared without a purpose.
Evidence retention and access policy are part of the trust agreement.
