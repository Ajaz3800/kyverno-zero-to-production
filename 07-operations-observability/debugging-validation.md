# 5. debugging-validation

## Goal

Troubleshoot validation policy failures.

---

# Common Problems

- policy not matching
- incorrect patterns
- namespace exclusions
- invalid JMESPath
- webhook failures

---

# Useful Commands

## Describe Policy

```bash
kubectl describe cpol <policy-name>
```

---

## View Events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Check Kyverno Logs

```bash
kubectl logs -n kyverno deploy/kyverno-admission-controller
```

---

# Production Tip

Always validate:
- match blocks
- namespace selectors
- admission operations

---