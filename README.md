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
