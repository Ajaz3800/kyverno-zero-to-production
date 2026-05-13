# 7. generate-rules

## Goal

Automatically generate Kubernetes resources.

## Common Generate Use Cases

- NetworkPolicies
- ResourceQuotas
- LimitRanges
- Secrets
- ConfigMaps

---

# Generate Flow

```text
Namespace Created
      ↓
Kyverno Generate Rule
      ↓
Security Resources Created Automatically
```

---