# 3. jmespath

## Goal

Use JMESPath expressions for advanced data extraction.

---

# What Is JMESPath?

JMESPath is a query language used in Kyverno to:
- access request data
- filter arrays
- evaluate conditions dynamically

---

# Example

```yaml
{{ request.object.spec.containers[].image }}
```

---

# Common Use Cases

- Inspect container images
- Validate labels
- Check annotations
- Process arrays dynamically

---

# Production Importance

JMESPath powers:
- dynamic policies
- advanced validation
- conditional logic

---