# Kyverno Operations, Monitoring & Troubleshooting

This section covers production operations, monitoring, debugging, scalability, and operational management.

These concepts are critical for:
- enterprise Kubernetes operations
- platform engineering
- DevSecOps observability
- production reliability
- policy troubleshooting
- high availability

---

# Directory Structure

```text
07-operations-and-monitoring/
├── monitoring.md
├── prometheus-integration.md
├── grafana-dashboards.md
├── policyreports.md
├── debugging-validation.md
├── debugging-mutation.md
├── admission-latency.md
├── performance-tuning.md
├── high-availability.md
├── upgrades.md
├── disaster-recovery.md
├── alerting.md
└── operational-runbooks.md
```

---

# Very Important Production Lessons

## 1. Kyverno Is Critical Infrastructure

Kyverno directly affects:
- deployments
- admission requests
- cluster operations

Treat it like:
- production platform infrastructure

---

## 2. Poor Policies Cause Performance Problems

Bad policies can:
- increase API latency
- slow deployments
- overload controllers

---

## 3. Monitoring Is Mandatory

Without monitoring:
- failures become invisible
- latency problems go unnoticed

---

## 4. GitOps Makes Recovery Easier

Store:
- policies
- dashboards
- alerts
- configurations

inside Git.

---

## 5. HA Is Required In Production

Single-replica deployments create:
- operational risk
- admission outages

---

## 6. Audit Before Enforce

Always start new policies with:

```yaml
validationFailureAction: Audit
```

before:
- Enforce

---

# Recommended Learning Order

1. monitoring
2. prometheus-integration
3. grafana-dashboards
4. policyreports
5. debugging-validation
6. debugging-mutation
7. admission-latency
8. performance-tuning
9. alerting
10. high-availability
11. upgrades
12. disaster-recovery
13. operational-runbooks

---

# Useful Commands

## Check Kyverno Pods

```bash
kubectl get pods -n kyverno
```

---

## View Logs

```bash
kubectl logs -n kyverno deploy/kyverno-admission-controller
```

---

## View Policy Reports

```bash
kubectl get polr -A
```

---

## View Events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Check Metrics

```bash
kubectl port-forward svc/kyverno-svc-metrics -n kyverno 8000:8000
```

---

# References

- Official Kyverno Documentation:
  https://kyverno.io

- Prometheus:
  https://prometheus.io

- Grafana:
  https://grafana.com

- Kubernetes Admission Controllers:
  https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/