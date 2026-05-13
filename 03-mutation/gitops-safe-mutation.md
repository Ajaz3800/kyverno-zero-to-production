# 10. gitops-safe-mutation

## Goal

Understand GitOps-safe mutation strategies.

---

# VERY Important Production Topic

Mutation can conflict with tools like:

- ArgoCD
- FluxCD

Because:
- Git desired state
- Cluster mutated state

may differ.

This creates:
- drift detection issues
- continuous reconciliation loops

---

# Example Drift Scenario

```text
Git Manifest
     ↓
No Label Present
     ↓
Kyverno Adds Label
     ↓
ArgoCD Detects Drift
```

---