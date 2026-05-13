# 2. prometheus-integration

## Goal

Integrate Kyverno metrics with Prometheus.

---

# Why Important

Kyverno exposes Prometheus metrics for:
- admission latency
- policy execution
- webhook requests
- controller performance

---

# Common Metrics

```text
kyverno_admission_review_duration_seconds
kyverno_policy_rule_results_total
kyverno_http_requests_total
```

---

# Verify Metrics Endpoint

```bash
kubectl port-forward svc/kyverno-svc-metrics -n kyverno 8000:8000
```

---

# Access Metrics

```text
http://localhost:8000/metrics
```

---