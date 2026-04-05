# ADR-003: Secure Access Model – VPN-Based Zero Trust Administration

## Status
Accepted

## Date
2026-03-26

---

## Context

The platform requires secure administrative access to sensitive components such as:

- Kubernetes API
- Argo CD
- Grafana (observability)
- Internal services

Traditional approaches often expose these services via public endpoints secured by authentication mechanisms.

However, exposing administrative interfaces to the public internet increases:

- Attack surface
- Risk of misconfiguration
- Dependency on perimeter security

The platform is designed with a security-first and sovereignty-focused mindset, where minimizing exposure is a core principle.

---

## Decision

Adopt a VPN-based access model using NetBird for all administrative access.

### Key aspects:

- All admin-level services are placed in a PrivateZone
- No direct public exposure of:
  - Kubernetes API
  - Argo CD
  - Grafana
- Access is only possible through an authenticated VPN connection
- VPN integrates with the identity provider (Keycloak)

---

## Architecture

- NetBird VPN acts as the entry point for administrative access
- Only authenticated and authorized users can access internal services
- PublicZone is strictly limited to user-facing services via controlled ingress

---

## Alternatives Considered

### Public Exposure with Authentication (Rejected)

Pros:
- Simpler access (no VPN required)
- Easier onboarding

Cons:
- Increased attack surface
- Relies heavily on correct configuration of each service
- Higher risk of credential attacks and scanning

---

### Bastion Host (Rejected)

Pros:
- Centralized access point
- Common enterprise pattern

Cons:
- Adds operational overhead
- Still exposes an entry point publicly
- Less flexible compared to mesh VPN

---

### Full Mesh without Identity Integration (Rejected)

Pros:
- Simpler setup

Cons:
- No centralized identity control
- Harder to manage access policies
- Not aligned with identity-first design

---

## Consequences

### Positive

- Strong reduction of public attack surface
- Centralized access control via identity provider
- Clear separation between public and private services
- Aligns with zero trust principles
- Improved auditability and control

---

### Negative

- Requires VPN setup for administrators
- Slightly higher onboarding complexity
- Dependency on VPN availability for admin access

---

## Future Considerations

- Integrate fine-grained access policies via identity roles
- Evaluate device posture checks (e.g. trusted devices)
- Implement audit logging for access via VPN
- Consider high availability setup for NetBird control plane

---

## Summary

Administrative access is restricted to a private network via VPN.

This decision enforces a zero trust-inspired model where:

- No sensitive services are exposed publicly
- Identity and access control are centralized
- The platform minimizes its external attack surface

This aligns with the overall goal of building a secure, sovereign, and controlled platform.