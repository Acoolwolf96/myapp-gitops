# GitOps Portfolio with Argo CD & K3s

This project demonstrates a complete GitOps-based deployment pipeline running on a 
self-hosted Kubernetes cluster, with a private container registry and layered network 
security. It shows how deployments can be automated, version-controlled and self-healing 
without dependency on external image registries.

---



## Architecture Overview

GitHub Repository → Argo CD → K3s Cluster → Deployment → Pods (nginx) → Service → User

---

## Infrastructure

- **Hypervisor:** Proxmox VE on HP EliteDesk 800 G1 SFF
- **K3s Node:** Ubuntu Server 24.04 VM
- **Remote Access:** Tailscale WireGuard mesh VPN
- **Host Firewall:** UFW — default deny, Tailscale-only access

---

## Application

A simple static portfolio website served with nginx.

- **Base Image:** nginx:alpine
- **Image Registry:** Self-hosted private Docker Registry v2
- **Replicas:** 3
- **Deployment Strategy:** Rolling updates (zero downtime)

---

## Private Container Registry

Images are built locally and pushed to a self-hosted registry running inside K3s.
This eliminates Docker Hub dependency and keeps the entire pipeline within the 
local network.

- **Storage:** Persistent volume on host disk
- **Access:** Local network only

---

## Security Model

| Layer | Tool | What it does |
|-------|------|-------------|
| 1 | Tailscale | Encrypted WireGuard mesh — no ports exposed to internet |
| 2 | Router NAT | No port forwarding — homelab invisible to public internet |
| 3 | UFW | Default-deny host firewall — SSH and NodePorts via Tailscale only |
| 4 | K3s networking | Pod and service network isolation |

---

## GitOps Workflow

1. Code change is made and Docker image is built locally
2. Image is pushed to private self-hosted registry
3. Deployment manifest is updated in this GitHub repository
4. Argo CD detects the change automatically
5. Argo CD syncs the new manifest to the K3s cluster
6. Kubernetes performs a rolling update — zero downtime
7. Self-healing: if cluster state drifts, Argo CD reconciles automatically

---

## Services

| Service | Type |
|---------|------|
| Portfolio app | NodePort |
| Argo CD UI | NodePort |
| Private Registry | NodePort |

---

## Argo CD Dashboard
*Argo CD continuously synchronizing the cluster with the GitHub repository.*
<img width="1920" height="1042" alt="image" src="https://github.com/user-attachments/assets/621ad916-780f-4fd3-9392-cab37f639136" />


