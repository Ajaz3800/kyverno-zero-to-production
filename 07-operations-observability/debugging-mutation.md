# 6. debugging-mutation

## Goal

Troubleshoot mutation policies.

---

# Common Mutation Issues

- mutation not applied
- GitOps drift
- incorrect strategic merge
- foreach errors
- admission ordering

---

# Verify Mutation

```bash
kubectl get pod <pod-name> -o yaml
```

---

# Production Tip

Use:
- additive mutation
- `+()` anchors

for safer GitOps compatibility.

---