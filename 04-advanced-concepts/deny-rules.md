# 2. deny-rules

## Goal

Block resources dynamically using conditions.

---

# Why Important

`deny` rules provide:
- fine-grained security enforcement
- dynamic validation
- complex admission logic

---

# Example

```yaml
deny:
  conditions:
    any:
      - key: "{{ request.object.spec.hostNetwork }}"
        operator: Equals
        value: true
```

---

# Common Use Cases

- Block privileged containers
- Restrict host networking
- Prevent dangerous capabilities
- Block specific image tags

---

# Production Recommendation

Use `deny` when:
- pattern matching becomes complex
- dynamic conditions are needed

---