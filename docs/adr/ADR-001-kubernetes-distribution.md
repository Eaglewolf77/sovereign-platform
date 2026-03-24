# ADR-001: Kubernetes Distribution

## Status
Proposed

## Context

The platform requires a Kubernetes distribution that supports:

- On-premise deployment on Hyper-V
- GitOps-driven operations (ArgoCD)
- Low operational overhead due to limited available time
- Strong security posture aligned with zero-trust principles
- Compatibility with Cilium (CNI), Longhorn (storage), and other cloud-native components
- Long-term maintainability and scalability

The platform is intended to evolve into a sovereign, identity-first system with minimal manual intervention.

Key requirements:

- Minimal configuration drift
- Declarative management where possible
- Stable and predictable upgrades
- Good support for OIDC integration and RBAC

The following options are considered:

- k3s
- Talos Linux (with Kubernetes)
- kubeadm-based cluster

---

## Decision

Talos Linux is selected as the Kubernetes distribution for the platform.

---

## Rationale

Talos Linux provides:

- Immutable, API-driven operating system
- No SSH access, reducing attack surface
- Fully declarative configuration model
- Strong alignment with GitOps principles
- Reduced operational overhead once deployed
- Predictable upgrades and lifecycle management

This aligns closely with the platform principles:

- Identity-first: Reduced reliance on node-level access
- GitOps-driven: Declarative infrastructure and cluster state
- Secure-by-default: Minimal OS surface and enforced patterns
- Low operational overhead: Simplified maintenance model

---

## Alternatives Considered

### k3s

Pros:
- Lightweight and easy to bootstrap
- Lower initial complexity
- Well-suited for small environments

Cons:
- Less strict operational model
- Allows configuration drift more easily
- Relies on traditional OS management (SSH, package updates)

### kubeadm

Pros:
- Maximum flexibility
- Upstream Kubernetes experience

Cons:
- High operational overhead
- Manual lifecycle management
- Increased risk of configuration drift
- Not aligned with low-maintenance goal

---

## Consequences

### Positive

- Strong alignment with platform principles
- Reduced attack surface (no SSH)
- Declarative and reproducible cluster management
- Better long-term maintainability

### Negative

- Steeper learning curve
- More complex initial setup
- Smaller community compared to k3s

### Neutral

- Requires learning Talos-specific tooling and workflows

---

## Notes

This decision prioritizes long-term maintainability and architectural consistency over initial simplicity.

Future ADRs will build on this decision, including:

- Networking model (Cilium configuration)
- Identity integration (OIDC with Kubernetes API)
- GitOps repository structure
