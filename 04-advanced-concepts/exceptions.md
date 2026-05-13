# 6. exceptions

## Goal

Exclude workloads from policy enforcement safely.

---

# Why Important

Production clusters often require exemptions for:
- platform workloads
- monitoring systems
- ingress controllers
- legacy applications

---

# Example

```yaml
exclude:
  any:
    - resources:
        namespaces:
          - kube-system
          - monitoring
```

---

# Production Best Practice

Always document:
- who requested exception
- why exception exists
- expiration timeline

---