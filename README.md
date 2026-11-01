# argo-demo-config
# ☸️ ArgoCD GitOps Demo - Infrastructure Manifests (`argo-demo-config`)

This repository serves as the **Absolute Single Source of Truth** for the deployment state of our infrastructure. It contains purely declarative Kubernetes manifests managed via GitOps methodology.

## 🚀 GitOps Architecture Loop
Instead of traditional "push-based" deployment scripts (e.g., executing `kubectl apply` from an external CI server), this project implements a **pull-based GitOps loop** powered by **Argo CD**:
1. **State Observation:** Argo CD runs directly inside the Kubernetes cluster and continuously monitors this repository.
2. **Drift Detection:** If any configuration shifts in Git, or if manual changes are erroneously applied to the cluster, Argo CD detects the discrepancy.
3. **Automated Reconciliation:** The cluster state is automatically self-healed to strictly match the declarative definitions stored here.

## 📁 Repository Directory Structure
```text
argo-demo-config/
├── manifests/
│   ├── deployment.yaml   # Defines application replicas, resources, and image tags
│   └── service.yaml      # Manages internal network routing and external node mapping
└── README.md
```

## 🚀 Cluster Bootstrapping

To provision this application into your local Kubernetes cluster (Minikube / Docker Desktop):

### 1. Initialize the Argo CD Operator
```bash
# Create the isolated namespace
kubectl create namespace argocd

# Apply the official upstream Argo CD core manifests
kubectl apply -n argocd -f https://githubusercontent.com
```

### 2. Deploy the Declarative Application Blueprint
Apply the root application manifest to instruct Argo CD to track this repository:

```bash
kubectl apply -f https://githubusercontent.com
```

## 🛡️ Production DevOps Best Practices Demonstrated
* **Immutability:** Deployment changes are tracked via unique Git Commit SHAs rather than mutable `latest` tags.
* **Audit Trail:** Every single deployment, rollback, and configuration change leaves a permanent cryptographically signed trail in Git history.
* **Zero Cluster Access Required for CI:** The application builder pipeline needs zero administrative access tokens to the live Kubernetes API, significantly shrinking the attack surface.
