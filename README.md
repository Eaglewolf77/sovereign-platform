
# Sovereign Platform

A sovereign, identity-first platform built with Kubernetes, GitOps, and Infrastructure as Code.

Designed to eliminate hyperscaler dependencies while enforcing Zero Trust and full control over identity, infrastructure, and data.

---

## 🧠 Core Principles

- Identity-first architecture (OIDC / Keycloak)
    
- Zero Trust security model
    
- Self-hosted infrastructure (no hyperscaler dependency)
    
- Declarative infrastructure and GitOps workflows
    
- Observability and reliability by design
    
- Separation of control plane components
    

---

## 🏗️ Architecture Overview

This platform is structured around clear trust boundaries:

- **Public Zone** – Internet-facing services via Cilium Gateway API
    
- **Private Zone** – Administrative access via NetBird VPN
    
- **Internal Zone** – Kubernetes cluster and storage layer
    
- **Identity Layer** – External Keycloak instance acting as trust anchor
    

Key design decisions:

- Externalized identity provider (Keycloak outside Kubernetes)
    
- Secure access via VPN (no direct public control plane exposure)
    
- eBPF-based networking using Cilium
    
- Distributed storage using LINSTOR
    
- GitOps-driven deployments with Argo CD
    

---

## ⚙️ Core Components

- Kubernetes (kubeadm-based cluster provisioning)
    
- Cilium (eBPF networking & Gateway API)
    
- LINSTOR (distributed storage layer)
    
- Keycloak (external identity provider)
    
- Argo CD (GitOps)
    
- Velero (backup & disaster recovery)
    
- NetBird (secure administrative access)
    
- Grafana (observability)
    

---

## 🔐 Security Model

- Identity-first authentication (OIDC)
    
- Zero Trust networking principles
    
- Segmented network zones (Public / Private / Internal)
    
- No direct access to Kubernetes API from the public internet
    
- VPN-based administrative access
    

---

## 🧱 Storage Architecture

Storage is handled outside Kubernetes via LINSTOR:

- Decoupled storage layer
    
- Multi-node replication
    
- Designed for resilience and sovereignty
    

---

## 📜 Architecture Decisions

Architecture Decision Records (ADR) are documented in:

```
/docs/adr
```

These documents describe design choices, trade-offs, and reasoning behind the platform.

---

## 🚧 Project Status

Work in progress — currently focused on architecture, design decisions, and platform foundation.

---

## 🎯 Purpose

This project is part of a long-term initiative to design and build a sovereign, identity-centric platform aligned with modern platform engineering and security principles.