# ADR-000: Platform Scope and Direction

## Status
Accepted

## Context

This project aims to design and build a sovereign, self-hosted platform with a strong focus on identity, security, and full infrastructure control.

The goal is to explore modern platform engineering practices without relying on hyperscaler-managed services, while still aligning with enterprise architecture principles.

## Decision

The platform will be designed as an identity-first system, where authentication, authorization, and access control are central to all components.

The architecture will prioritize:

- Self-hosted infrastructure
- Declarative configuration (Infrastructure as Code)
- GitOps workflows
- Zero Trust security principles
- Observability and resilience

Proxmox and similar virtualization platforms were evaluated as a base layer for infrastructure.

However, for this phase of the project, the focus is on higher-level platform design rather than underlying virtualization.

This allows exploration of:

- Kubernetes as a platform layer
- Identity integration (OIDC / Keycloak)
- Secure service-to-service communication
- Platform-level automation and operations

## Consequences

- Increased complexity compared to managed cloud services
- Full control over identity, data, and infrastructure
- Better alignment with sovereign and compliance-driven environments (e.g. GDPR, Schrems II)
- Strong foundation for future hybrid or on-prem extensions
