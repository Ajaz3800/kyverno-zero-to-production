# 5. anchors

## Goal

Control matching and mutation behavior precisely.

---

# Common Anchors

| Anchor | Purpose |
|---|---|
| `()` | Conditional anchor |
| `=` | Equality anchor |
| `^()` | Existence anchor |
| `+()` | Add-if-not-present |

---

# Example

```yaml
+(environment): production
```

---

# Why Important

Anchors improve:
- safe mutation
- GitOps compatibility
- conditional matching

---

# Production Recommendation

Prefer:

```yaml
+()
```

for GitOps-safe mutation.

---