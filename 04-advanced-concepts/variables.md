# 4. variables

## Goal

Use dynamic variables inside policies.

---

# Common Variables

```yaml
{{ request.object.metadata.name }}
{{ request.namespace }}
{{ request.userInfo.username }}
```

---

# Why Important

Variables help create:
- reusable policies
- dynamic validation
- user-aware rules

---

# Common Use Cases

- Namespace-aware policies
- User-based restrictions
- Dynamic mutation
- Audit metadata injection

---