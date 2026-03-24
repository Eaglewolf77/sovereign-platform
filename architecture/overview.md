## High-Level Architecture

The platform is composed of:

- A Kubernetes cluster provisioned via OpenTofu on Hyper-V
- GitOps-driven deployment using ArgoCD
- Cilium for networking and ingress control
- Longhorn for distributed storage
- Keycloak as the identity provider (OIDC)
- Velero for backup and restore
- NetBird VPN for secure operator access
- DNS managed externally (one.com)

Applications (e.g., Nextcloud, Collabora) run inside the cluster and rely on platform services.
