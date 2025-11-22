#  Kubernetes Homelab

> Production-grade Kubernetes infrastructure implementing GitOps workflows, encrypted secrets management, and secure external access via Cloudflare Tunnels.

## Overview

This repository contains the complete Infrastructure as Code (IaC) for my self-hosted Kubernetes homelab. Built on **K3s** running on **Proxmox VMs**, this setup demonstrates enterprise-level DevOps practices including GitOps automation with FluxCD, secrets encryption with SOPS, and comprehensive observability.

## Principles

- **GitOps-First**: All infrastructure and application changes are version-controlled and automatically deployed via FluxCD
- **Security by Default**: All secrets encrypted with SOPS/AGE, no plaintext credentials in Git
- **Zero-Trust Networking**: External access secured through Cloudflare Tunnels, no port forwarding
- **Observability**: Full monitoring stack with Prometheus and Grafana
- **Declarative Configuration**: 100% Infrastructure as Code - no manual kubectl commands in production

## Tech Stack

| Component | Technology |
|-----------|-----------|
| **Kubernetes Distribution** | K3s |
| **Infrastructure** | Proxmox VE |
| **GitOps** | FluxCD |
| **Secrets Management** | SOPS + AGE |
| **Ingress Controller** | Traefik |
| **Secure Access** | Cloudflare Tunnels |
| **Monitoring** | Prometheus + Grafana |
| **Dependency Management** | Renovate |

## Applications

Currently self-hosting:

- **[Linkding](https://github.com/sissbruecker/linkding)** - Bookmark manager
- **[Audiobookshelf](https://www.audiobookshelf.org/)** - Audiobook and podcast server
- **Prometheus + Grafana** - Monitoring and observability stack

All applications are:

- Deployed declaratively via Kustomize manifests
- Automatically synced from this Git repository
- Secured with encrypted TLS certificates
- Accessible externally via Cloudflare Tunnels

## Repository Structure
```
.
├── apps/
│   ├── base/                 # Base application configurations
│   └── staging/              # Environment-specific overlays
├── clusters/
│   └── saltechng/           # Cluster bootstrap configuration
├── infrastructure/
│   └── controllers/         # Infrastructure controllers (Renovate, etc.)
└── monitoring/
    ├── configs/             # Monitoring configurations
    └── controllers/         # Prometheus operator deployment
```

Following the [FluxCD recommended structure](https://fluxcd.io/flux/guides/repository-structure/) with base and overlay patterns for environment separation.

## Key Features

### GitOps Automation

- **FluxCD** continuously monitors this repository and automatically applies changes
- Pull-based model - cluster pulls configuration, nothing pushes to cluster
- Self-healing - if manual changes are made, Flux reverts to Git state

### Secrets Management

- All secrets encrypted with **SOPS** using **AGE** encryption
- Private keys stored only in cluster (not in Git)
- Flux automatically decrypts secrets during deployment
- Zero plaintext credentials in version control

### Secure External Access

- **Cloudflare Tunnels** provide secure access without exposing home IP
- No port forwarding required
- Zero-trust network access
- Automatic HTTPS with Cloudflare-managed certificates

### Monitoring & Observability

- **Prometheus** for metrics collection
- **Grafana** for visualization and dashboards
- Full cluster and application monitoring

### Automated Maintenance

- **Renovate** automatically creates PRs for dependency updates
- Keeps all applications and infrastructure components up-to-date
- Automated security patching

## Getting Started

### Prerequisites

- Kubernetes cluster (K3s recommended)
- FluxCD CLI installed
- SOPS with AGE configured
- GitHub repository access

### Bootstrap

1. **Install Flux:**
```bash
flux bootstrap github \
  --owner=Deyinka07 \
  --repository=cluster \
  --branch=main \
  --path=clusters/saltechng
```

2. **Configure SOPS:**
```bash
# Generate AGE key
age-keygen -o age.agekey

# Create Flux secret
kubectl create secret generic sops-age \
  --namespace=flux-system \
  --from-file=age.agekey=./age.agekey
```

3. **Flux will automatically:**
   - Deploy all infrastructure controllers
   - Set up monitoring stack
   - Deploy applications
   - Configure ingress and certificates

## Current Goals

- [ ] Implement automated backup strategy for persistent data
- [ ] Expand monitoring with custom Grafana dashboards
- [ ] Deploy additional self-hosted applications
- [ ] Add comprehensive disaster recovery procedures
- [ ] Document architecture decisions and setup process
- [ ] Implement resource requests/limits optimization

## Recently Completed

- [x] Automated dependency updates with Renovate
- [x] Cloudflare Tunnel integration for secure external access
- [x] SOPS encryption for secrets management
- [x] Full monitoring stack deployment

## Architecture

**Compute:** K3s cluster running on Proxmox VMs  
**Networking:** Traefik ingress with Cloudflare Tunnels  
**Storage:** Local path provisioner  
**GitOps:** FluxCD with automatic reconciliation  
**Security:** SOPS-encrypted secrets, non-root containers, network policies

## Lessons Learned

Building this homelab taught me:

- Production GitOps workflows and best practices
- Kubernetes secrets management at scale
- Troubleshooting complex networking and ingress issues
- Infrastructure as Code principles
- Importance of proper monitoring and observability

