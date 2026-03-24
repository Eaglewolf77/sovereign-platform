# ADR-001: Kubernetes Distribution

## Status
Accepted

## Context

The platform requires a Kubernetes distribution that supports:

- On-premise deployment on Hyper-V
- GitOps-driven operations (ArgoCD)
- Compatibility with Cilium (CNI), Longhorn (storage), and other cloud-native components
- Identity-first architecture with external OIDC provider (Keycloak)
- Incremental, step-by-step development
- Full control over system components for learning and debugging

The platform is developed as a long-term project with a focus on understanding, maintainability, and architectural clarity.

Options considered:

- Kubernetes on AlmaLinux (kubeadm-based)
- k3s
- Talos Linux

---

## Decision

Kubernetes will be deployed on AlmaLinux using a kubeadm-based setup.

---

## Rationale

AlmaLinux with kubeadm provides:

- Full control over the operating system and Kubernetes components
- Standard, upstream Kubernetes experience
- Familiar operational model (SSH, systemd, package management)
- Flexibility for troubleshooting and iterative development

This aligns with the current phase of the platform:

- Prioritizes learning and transparency over abstraction
- Enables step-by-step implementation
- Avoids hidden complexity from opinionated distributions

---

## Alternatives Considered

### k3s

Pros:
- Lightweight and fast to deploy
- Reduced operational complexity

Cons:
- Abstracts away Kubernetes internals
- Less control over components

---

### Talos Linux

Pros:
- Immutable and secure
- Strong alignment with GitOps and zero-trust

Cons:
- Steep learning curve
- Limited flexibility for debugging
- Higher initial complexity

---

## Consequences

### Positive

- Full visibility and control over the platform
- Easier debugging and troubleshooting
- Strong foundation for learning Kubernetes internals

### Negative

- Higher operational overhead
- Increased risk of configuration drift
- Manual responsibility for OS and cluster lifecycle management

### Neutral

- Future migration to a more opinionated platform (e.g., Talos) remains possible

---

## Notes

To mitigate operational risks:

- Node configuration should be automated (future ADR)
- GitOps must be strictly enforced for workloads
- OS hardening and patching strategy should be defined
