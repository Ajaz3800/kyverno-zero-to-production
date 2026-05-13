# 1. preconditions

## Goal

Apply rules only when specific conditions match.

---

# Why Important

Preconditions help:
- avoid unnecessary policy execution
- reduce false positives
- scope policies dynamically

---

# Example

```yaml
preconditions:
  all:
    - key: "{{ request.operation }}"
      operator: Equals
      value: CREATE
```

---

# Common Use Cases

- Only validate CREATE requests
- Skip DELETE operations
- Target specific labels
- Apply rules only for certain users

---

# Production Benefit

Preconditions improve:
- performance
- policy flexibility
- operational safety

---