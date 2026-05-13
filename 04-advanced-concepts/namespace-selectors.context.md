# 7. namespace-selectors

## Goal

Apply policies dynamically using namespace labels.

---

# Example

```yaml
namespaceSelector:
  matchLabels:
    environment: production
```

---

# Why Important

Namespace selectors enable:
- scalable governance
- multi-tenant policy management
- environment-based enforcement

---

# Production Benefit

Avoid hardcoding namespace names.

Prefer:
- labels
- selectors
- dynamic grouping

---