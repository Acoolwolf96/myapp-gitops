# GitOps Portfolio with Argo CD & K3s

This project demonstrates a complete GitOps-based deployment pipeline using Argo CD and K3s on a bare-metal setup.

It shows how deployments can be automated, and version-controlled

---

## Architecture Overview

GitHub Repository → Argo CD → K3s Cluster → Deployment → Pods (nginx) → Service → User

---

## Application

A simple static portfolio website served with nginx.

- Base Image: nginx:alpine  
- Image Registry: Docker Hub
- Replicas(containers)
- Deployment Strategy: Rolling updates    

---

## GitOps Workflow

1. Changes are pushed to GitHub  
2. Argo CD detects changes in the repository  
3. Argo CD syncs with the cluster 
4. Kubernetes performs a rolling update  
5. Application is updated with zero downtime  
