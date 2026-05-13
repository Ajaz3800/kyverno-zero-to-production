# 4. gitops-integration

## Goal

Integrate Kyverno with GitOps workflows.

---

# GitOps Workflow

```text
Git Repository
      ↓
ArgoCD Sync
      ↓
Kubernetes Cluster
      ↓
Kyverno Admission Policies
```

---

# Why Important

GitOps provides:
- version control
- auditability
- rollback capability
- declarative management

---

# Production Recommendation

Always:
- test policies in lower environments
- validate ArgoCD sync behavior
- monitor reconciliation loops

---