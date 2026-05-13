# Kyverno Operations, Monitoring & Troubleshooting

This section covers production operations, monitoring, debugging, scalability, and operational management for :contentReference[oaicite:0]{index=0}.

These concepts are critical for:
- enterprise Kubernetes operations
- platform engineering
- DevSecOps observability
- production reliability
- policy troubleshooting
- high availability

---

# 1. monitoring.md

## Goal

Monitor Kyverno health and operational status.

---

# Why Monitoring Matters

Kyverno operates inline with:
- Kubernetes API requests
- admission webhooks
- policy processing

Monitoring helps detect:
- policy failures
- webhook latency
- controller crashes
- scaling issues

---

# Key Areas To Monitor

- webhook latency
- admission request volume
- policy violations
- controller health
- CPU/memory usage

---

# Recommended Stack

- :contentReference[oaicite:1]{index=1}
- :contentReference[oaicite:2]{index=2}
- Alertmanager

---

# 2. prometheus-integration.md

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

# 3. grafana-dashboards.md

## Goal

Visualize Kyverno operational metrics.

---

# Recommended Dashboards

- admission latency
- policy violations
- webhook failures
- controller resource usage
- request throughput

---

# Production Benefits

Dashboards help:
- identify bottlenecks
- detect policy failures
- monitor cluster health

---

# Recommended Panels

| Panel | Purpose |
|---|---|
| Admission Latency | Detect slow policies |
| Policy Violations | Compliance visibility |
| Controller Restarts | Stability monitoring |
| API Throughput | Capacity planning |

---

# 4. policyreports.md

## Goal

Understand PolicyReports operationally.

---

# What Are PolicyReports?

PolicyReports store:
- policy violations
- audit results
- compliance data

---

# View Reports

```bash
kubectl get polr -A
```

---

# Describe Report

```bash
kubectl describe polr <report-name> -n <namespace>
```

---

# Common Report Results

| Result | Meaning |
|---|---|
| pass | Policy satisfied |
| fail | Policy violated |
| warn | Warning generated |
| error | Policy execution issue |
| skip | Rule skipped |

---

# 5. debugging-validation.md

## Goal

Troubleshoot validation policy failures.

---

# Common Problems

- policy not matching
- incorrect patterns
- namespace exclusions
- invalid JMESPath
- webhook failures

---

# Useful Commands

## Describe Policy

```bash
kubectl describe cpol <policy-name>
```

---

## View Events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Check Kyverno Logs

```bash
kubectl logs -n kyverno deploy/kyverno-admission-controller
```

---

# Production Tip

Always validate:
- match blocks
- namespace selectors
- admission operations

---

# 6. debugging-mutation.md

## Goal

Troubleshoot mutation policies.

---

# Common Mutation Issues

- mutation not applied
- GitOps drift
- incorrect strategic merge
- foreach errors
- admission ordering

---

# Verify Mutation

```bash
kubectl get pod <pod-name> -o yaml
```

---

# Production Tip

Use:
- additive mutation
- `+()` anchors

for safer GitOps compatibility.

---

# 7. admission-latency.md

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

# 8. performance-tuning.md

## Goal

Optimize Kyverno performance.

---

# Optimization Strategies

## 1. Scope Policies Carefully

Avoid:
- unnecessary cluster-wide matching

---

## 2. Use Preconditions

Prevent unnecessary rule execution.

---

## 3. Reduce External Context Calls

API lookups increase latency.

---

## 4. Avoid Expensive JMESPath

Complex queries reduce throughput.

---

## 5. Monitor Controller Resources

Watch:
- CPU
- memory
- restarts

---

# Production Recommendation

Scale Kyverno according to:
- admission request volume
- cluster size
- policy complexity

---

# 9. high-availability.md

## Goal

Deploy Kyverno in high availability mode.

---

# Why Important

Kyverno is critical infrastructure.

If unavailable:
- admission may fail
- deployments may stop

---

# HA Best Practices

- multiple replicas
- anti-affinity
- PodDisruptionBudgets
- resource requests/limits

---

# Example

```yaml
replicaCount: 3
```

---

# Production Recommendation

Never run:
- single-replica Kyverno

in production.

---

# 10. upgrades.md

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

# 11. disaster-recovery.md

## Goal

Prepare for operational failures.

---

# Important Backup Targets

- policies
- CRDs
- Git repositories
- admission configurations
- PolicyReports

---

# Production Recommendation

Store policies in:
- Git repositories

NOT only inside the cluster.

---

# Disaster Recovery Strategy

```text
Git Repository
      ↓
ArgoCD Restore
      ↓
Kyverno Policies Recreated
```

---

# 12. alerting.md

## Goal

Create operational alerts.

---

# Recommended Alerts

| Alert | Purpose |
|---|---|
| High Admission Latency | Detect performance issues |
| Controller Restarting | Detect instability |
| Webhook Failures | Detect admission problems |
| Policy Violations Spike | Detect security issues |

---

# Recommended Stack

- Prometheus
- Alertmanager
- Slack integration

---

# Production Tip

Avoid noisy alerts.

Focus on:
- actionable signals

---

# 13. operational-runbooks.md

## Goal

Create operational procedures for incidents.

---

# Recommended Runbooks

- Kyverno unavailable
- webhook timeout
- policy deployment rollback
- GitOps drift
- admission failures
- controller crash loops

---

# Example Incident Flow

```text
Alert Triggered
      ↓
Runbook Opened
      ↓
Root Cause Analysis
      ↓
Mitigation
      ↓
Postmortem
```

---

# Production Recommendation

Every critical alert should have:
- documented runbook
- escalation path
- rollback procedure

---

# Production Operations Architecture

```text
Kyverno
   ↓
Prometheus Metrics
   ↓
Grafana Dashboards
   ↓
Alertmanager
   ↓
Slack / Incident Response
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