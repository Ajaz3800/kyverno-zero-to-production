# 11. disaster-recovery

## Goal

Prepare for operational failures.

---

# Important Backup Targets

- policies
- CRDs
- Git repositories
- admission configurations
- PolicyReports

---

# Production Recommendation

Store policies in:
- Git repositories

NOT only inside the cluster.

---

# Disaster Recovery Strategy

```text
Git Repository
      ↓
ArgoCD Restore
      ↓
Kyverno Policies Recreated
```

---