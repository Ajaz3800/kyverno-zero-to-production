# 5. argocd-drift

## Goal

Understand mutation-related drift problems.

---

# What Is Drift?

Drift occurs when:
- Git desired state
- cluster actual state

are different.

---

# Example

```text
Git Manifest
     ↓
No Label
     ↓
Kyverno Mutation Adds Label
     ↓
ArgoCD Detects Drift
```

---

# Production Risks

- endless reconciliation loops
- unstable sync state
- operational confusion

---

# GitOps-Safe Mutation Strategy

Use:
- additive mutation
- +() anchors
- scoped mutation

Avoid:
- aggressive resource rewriting

---