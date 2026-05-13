# 10. upgrades

## Goal

Safely upgrade Kyverno versions.

---

# Upgrade Risks

Upgrades may affect:
- policy behavior
- CRDs
- admission logic
- webhook compatibility

---

# Production Upgrade Strategy

```text
Dev
 ↓
Staging
 ↓
Canary Production
 ↓
Full Production
```

---

# Recommended Practices

- backup policies
- review release notes
- test compatibility
- validate admission flow

---