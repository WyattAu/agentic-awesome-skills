Full reference for hardening workloads in Kubernetes; NetworkPolicy, RBAC, Pod Security Admission, admission controllers (Kyverno/OPA), and service account hardening.
2. [NetworkPolicy; Zero-Trust Networking](#networkpolicy)
3. [RBAC; Least Privilege](#rbac)
6. [Runtime Security; Falco](#runtime-security--falco)
# Audit before enforcing: find what would fail
## NetworkPolicy: Zero-Trust Networking
> **Prerequisite:** Your CNI plugin must support NetworkPolicy (Calico, Cilium, Weave Net; but NOT Flannel by default).
# Cilium: test connectivity between pods
# Calico: list effective policies
## RBAC: Least Privilege
# ❌ DANGEROUS: grants everything to everything
# ✅ CORRECT: minimal namespace-scoped role with specific resource names
Kyverno validates, mutates, and generates resources; no Rego knowledge required.
# ConstraintTemplate: define the Rego policy
# Constraint: apply the policy
    # EKS: IAM Roles for Service Accounts (IRSA)
    # GKE: Workload Identity
# In the pod spec: also disable token mounting
## Runtime Security: Falco
| **External Secrets Operator** | Sync from AWS Secrets Manager / GCP Secret Manager / Vault | Production; synchronizes Kubernetes Secrets, which are normally stored in etcd |
# External Secrets Operator: sync from AWS Secrets Manager