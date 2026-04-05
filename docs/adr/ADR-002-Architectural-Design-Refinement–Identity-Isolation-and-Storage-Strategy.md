
# ADR-002: Architectural Design Refinement – Identity Isolation and Storage Strategy

## Status
Accepted

## Date
2026-03-26

---

## Context

The platform architecture has evolved from an initial Kubernetes-centric design towards a more layered and infrastructure-oriented model.

Key observations during the design phase:

- Tight coupling between identity services and the Kubernetes cluster introduces unnecessary risk.
- Storage solutions tightly integrated into Kubernetes (e.g. Longhorn) abstract too much of the underlying infrastructure.
- The goal of the platform is to remain portable, sovereign, and independent of specific runtime environments.

The platform is designed to support:

- Identity-first architecture
- Clear separation of concerns
- Portability across environments (on-prem → cloud)
- Full control over infrastructure components

---

## Decision

The following architectural changes have been made:

### 1. Identity Layer Isolation

- Move Keycloak out of the Kubernetes cluster
- Deploy Keycloak on a dedicated virtual machine (VM)

This establishes identity as an independent foundational layer rather than a cluster-dependent service.

---

### 2. Storage Strategy Change

- Replace Longhorn with LINSTOR

Rationale:

- LINSTOR operates at the infrastructure level rather than inside Kubernetes
- Provides better alignment with an infra-first design
- Decouples storage from Kubernetes lifecycle

---

### 3. Network and Access Separation

- Introduce clear separation between:
  - PublicZone (internet-facing services)
  - PrivateZone (admin access via VPN)
  - InternalZone (cluster and infrastructure)

- Use NetBird VPN for all administrative access
- Avoid exposing Kubernetes API, ArgoCD, and observability tools publicly

---

### 4. Gateway and Ingress Control

- Use Cilium Gateway API for controlled ingress
- Route external traffic through a defined entry point
- Integrate with OIDC via Keycloak

---

## Alternatives Considered

### Longhorn (Rejected)

Pros:
- Easy Kubernetes-native deployment
- Popular and well-documented

Cons:
- Strong coupling to Kubernetes
- Less control over underlying storage behavior
- Not aligned with infra-first philosophy

---

### Keycloak inside Kubernetes (Rejected)

Pros:
- Simpler deployment model
- Unified management

Cons:
- Circular dependency (cluster depends on identity, identity depends on cluster)
- Increased blast radius during cluster failures
- Harder to recover independently

---

### Public Exposure of Admin Services (Rejected)

Pros:
- Easier access

Cons:
- Increased attack surface
- Violates zero-trust principles

---

## Consequences

### Positive

- Strong separation of concerns
- Improved security posture (reduced attack surface)
- Better portability across environments
- Easier disaster recovery (identity and storage decoupled)
- More predictable infrastructure behavior

---

### Negative

- Increased operational complexity
- More components to manage
- Requires clear documentation and automation (Ansible/OpenTofu)

---

## Future Considerations

- Automate full platform bootstrap (identity → network → cluster → apps)
- Introduce backup and recovery strategy for:
  - Keycloak VM
  - LINSTOR storage
- Evaluate multi-node control plane for higher availability
- Extend observability across all layers (not only Kubernetes)

---

## Summary

This decision formalizes a shift from a Kubernetes-centric design to a layered platform architecture:

- Identity is foundational and independent
- Storage is infrastructure-driven, not cluster-driven
- Access is controlled and segmented
- The platform is designed for portability and sovereignty

This aligns with the long-term goal of building a fully controlled, cloud-agnostic platform.
