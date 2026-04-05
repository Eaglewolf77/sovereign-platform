# ADR 004: Separation of Management and Platform Identity Planes

## Status
Proposed / Under Evaluation (Proof of Concept phase)

## Context
We are designing a strictly sovereign, Schrems II-compliant infrastructure platform. The platform requires robust identity and access management (IAM) for both end-users (Data Plane) and system administrators (Control Plane). 

Initially, the simplest approach would be to use a single Identity Provider (IdP) for all authentication. However, relying on the platform’s primary IdP for administrative VPN access (via Netbird) introduces a critical circular dependency: if the platform IdP goes offline, administrators cannot authenticate to the VPN to fix the issue. Furthermore, leveraging a US-based hyperscaler (e.g., Entra ID, Google Workspace) as a fallback is strictly prohibited by our data sovereignty requirements.

## Options Considered

1. **Single Keycloak Instance (Platform Hosted):** Use the main Keycloak cluster for both user OIDC and Netbird admin authentication.
   * *Pros:* Single source of truth, minimal operational overhead.
   * *Cons:* Severe circular dependency risk.

2. **Hyperscaler Managed IdP for Control Plane:** Use Entra ID or GitHub for Netbird authentication.
   * *Pros:* Highly available, zero local maintenance, solves circular dependency.
   * *Cons:* Violates Schrems II and data sovereignty principles.

3. **Split Identity Planes (Selected):** Deploy a lightweight, independent IdP (Authentik) on an isolated, EU-hosted node (Hetzner) exclusively for Netbird/Admin access, while keeping the primary Keycloak instance dedicated to platform workloads.

## Decision
We will implement **Option 3: Split Identity Planes**. 

Administrative access to the control plane will be governed by Authentik, hosted on an isolated Hetzner Cloud VM, integrated with Netbird. The primary platform workloads will utilize Keycloak. These systems will not depend on each other for uptime.

## Consequences

### Positive (The "Why")
* **Eliminates Circular Dependency:** Administrative access is completely decoupled from the platform's state. If the local Kubernetes cluster or Keycloak goes down, the control plane VPN remains accessible.
* **Strict Data Sovereignty:** By utilizing a European provider (Hetzner) and avoiding hyperscalers, the architecture remains fully Schrems II-compliant.
* **Reduced Blast Radius:** A compromise of the user-facing Keycloak instance does not grant access to the administrative infrastructure.
* **Resource Efficiency:** Using Authentik for the control plane avoids the heavy resource overhead of running a second full Keycloak cluster just for a handful of admins.

### Negative (The "Trade-offs")
* **Increased Operational Overhead:** The team must now maintain, patch, and monitor two distinct IdP systems (Authentik and Keycloak) instead of one.
* **Separation of State:** Admin accounts must be provisioned in a separate system, meaning there is no single pane of glass for all user and admin identities without complex federation.
* **Infrastructure Cost:** Requires maintaining external VM infrastructure (Hetzner) outside of the primary local hardware.
