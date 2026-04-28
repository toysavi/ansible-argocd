# 🚀 Ansible K3s GitOps Platform

This repository bootstraps a complete on-prem Kubernetes platform using:

- K3s (Lightweight Kubernetes)
- MetalLB (LoadBalancer for bare metal)
- Traefik (Ingress Controller)
- NFS (Persistent Storage + `/logs`)
- Redis (HA via Helm)
- ArgoCD (GitOps)
- Dex (SSO ready)

---

# 🧱 Architecture Overview
```
Users
↓
[ MetalLB External IP ]
↓
Traefik (Ingress)
↓
ArgoCD
↓
GitOps Apps (AWX, Monitoring, Logging, etc.)
↓
NFS Storage (PVC + /logs)
```

---

# 📁 Project Structure
```
ansible-k3s-gitops/
├── inventory/
├── playbooks/
├── roles/
├── group_vars/
└── README.md
```

---

# ⚙️ Prerequisites

- Ubuntu/Debian-based servers
- SSH access with sudo privileges
- Ansible installed on control node
- Network connectivity between nodes
- NFS server ready

---

# 🚀 Usage

## 6. Run It

Execute the full Ansible playbook to bootstrap your platform:

```bash
ansible-playbook -i inventory/prod.ini playbooks/site.yml

# ✅ 7. Verify

After deployment, validate cluster and components:
```
kubectl get nodes
kubectl get pods -A
kubectl get svc -A
```
# 🔑 Get ArgoCD Admin Password
```
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
``` 

# 🔧 Required Customization

Before running in production, make sure to update:

--- 

## FS Configuration
- nfs_server
- nfs_path

## LDAP / SSO (Dex)
- LDAP host
- bind DN / credentials
- user/group filters

## GitOps Repository
Replace:
```
https://your-git-repo/root-app.yaml
```
## Configure DNS
Replace: 
```
argocd.local
```
- Replace with a real domain in production
- Enable TLS using cert-manager