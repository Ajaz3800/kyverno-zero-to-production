# 8. context

## Goal

Fetch external data into policies.

---

# Context Sources

- ConfigMaps
- API calls
- Image registry metadata
- Kubernetes resources

---

# Example

```yaml
context:
  - name: nslabels
    apiCall:
      urlPath: "/api/v1/namespaces/{{request.namespace}}"
```

---

# Why Important

Context enables:
- dynamic decisions
- external validation
- advanced governance

---

# Production Warning

Context lookups can increase:
- admission latency
- webhook processing time

Use carefully.

---