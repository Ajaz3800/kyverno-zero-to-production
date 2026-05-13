# 7. admission-latency

## Goal

Understand Kyverno admission latency.

---

# Why Important

Kyverno runs inline with:
- Kubernetes API requests

Slow policies directly affect:
- deployments
- kubectl apply
- GitOps sync speed

---

# Common Causes

- too many policies
- external context calls
- complex JMESPath queries
- registry lookups
- image verification

---

# Recommended Monitoring

Monitor:

```text
kyverno_admission_review_duration_seconds
```

---

# Production Recommendation

Keep admission latency:
- as low as possible

especially in large clusters.

---