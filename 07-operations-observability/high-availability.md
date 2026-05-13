# 9. high-availability

## Goal

Deploy Kyverno in high availability mode.

---

# Why Important

Kyverno is critical infrastructure.

If unavailable:
- admission may fail
- deployments may stop

---

# HA Best Practices

- multiple replicas
- anti-affinity
- PodDisruptionBudgets
- resource requests/limits

---

# Example

```yaml
replicaCount: 3
```

---

# Production Recommendation

Never run:
- single-replica Kyverno

in production.

---