# Production Validation Policies

This section contains production-grade validation policies for :contentReference[oaicite:0]{index=0}.

These policies help enforce:
- Kubernetes security best practices
- workload compliance
- DevSecOps standards
- production reliability

---

# Policies Overview

| Policy | Purpose |
|---|---|
| require-non-root | Prevent containers from running as root |
| disallow-privileged | Block privileged containers |
| require-resources | Enforce CPU and memory requests/limits |
| require-probes | Require liveness and readiness probes |
| restrict-registries | Allow images only from approved registries |
| disallow-hostpath | Prevent dangerous hostPath mounts |
| require-labels | Enforce mandatory labels |
| rollout-strategy | Safe production rollout guidance |

---

# 1. require-non-root

## Goal

Ensure containers do not run as the root user.

## Why Important

Running containers as root increases:
- privilege escalation risks
- container breakout risks
- host compromise risks

## Production Best Practice

Use:

```yaml
securityContext:
  runAsNonRoot: true
```

---

# 2. disallow-privileged

## Goal

Block privileged containers.

## Why Important

Privileged containers can:
- access host devices
- bypass namespace isolation
- compromise cluster security

## Production Recommendation

Never allow:

```yaml
privileged: true
```

unless absolutely necessary.

---

# 3. require-resources

## Goal

Require CPU and memory requests/limits.

## Why Important

Without resource limits:
- noisy neighbor problems occur
- scheduling becomes unreliable
- nodes may become unstable

## Production Best Practice

Always define:

```yaml
resources:
  requests:
  limits:
```

---

# 4. require-probes

## Goal

Require:
- liveness probes
- readiness probes

## Why Important

Probes improve:
- self-healing
- traffic reliability
- deployment safety

## Production Best Practice

Every production workload should define:
- readinessProbe
- livenessProbe

---

# 5. restrict-registries

## Goal

Allow images only from trusted registries.

## Example

Allow only:
- ECR
- GCR
- ACR
- private registries

## Why Important

Prevents:
- untrusted images
- supply chain attacks
- malicious public images

---

# 6. disallow-hostpath

## Goal

Prevent use of `hostPath` volumes.

## Why Important

`hostPath` can expose:
- host filesystem
- Kubernetes node data
- container runtime files

This is considered high risk.

## Production Recommendation

Avoid `hostPath` whenever possible.

---

# 7. require-labels

## Goal

Enforce mandatory labels.

## Common Labels

```yaml
labels:
  app:
  environment:
  team:
  owner:
```

## Why Important

Labels help with:
- monitoring
- governance
- cost allocation
- automation
- observability

---

# 8. rollout-strategy

## Goal

Safely introduce policies into production.

---

# Recommended Rollout Pattern

```text
Audit
  ↓
Observe Violations
  ↓
Fix Workloads
  ↓
Gradual Namespace Rollout
  ↓
Enforce
```

---

# NEVER Start With Enforce

Bad Approach:

```yaml
validationFailureAction: Enforce
```

on day one.

This can:
- break deployments
- block CI/CD
- cause outages

---

# Recommended Approach

Start with:

```yaml
validationFailureAction: Audit
```

Observe:
- policy reports
- violations
- affected teams

Then gradually move to:
- Enforce

---

# Important Production Lessons

## 1. Policies Can Break Production

Always test in:
- dev
- staging
- pre-production

before cluster-wide rollout.

---

## 2. GitOps Drift Can Happen

Mutation policies can create drift with tools like :contentReference[oaicite:1]{index=1}.

Example:
- Git desired state differs from mutated cluster state.

---

## 3. Overly Broad Policies Are Dangerous

Avoid applying strict policies to:
- kube-system
- monitoring
- ingress controllers
- platform namespaces

without testing.

---

## 4. Monitor Admission Latency

Too many policies increase:
- API server latency
- webhook processing time

Monitor:
- admission webhook latency
- controller performance

---

## 5. Security Must Balance Reliability

Production security is NOT:
- blocking everything immediately

Production security IS:
- safe gradual enforcement
- operational stability
- controlled rollout

---

# Recommended Learning Order

1. require-non-root
2. require-resources
3. require-probes
4. require-labels
5. disallow-privileged
6. restrict-registries
7. disallow-hostpath
8. rollout-strategy

---

# Useful Commands

## List Policies

```bash
kubectl get cpol
```

---

## Describe Policy

```bash
kubectl describe cpol <policy-name>
```

---

## View Policy Reports

```bash
kubectl get polr -A
```

---

## Describe Policy Report

```bash
kubectl describe polr <report-name> -n <namespace>
```

---

## Check Kyverno Controllers

```bash
kubectl get pods -n kyverno
```

---

# References

- Official Kyverno Documentation:
  https://kyverno.io

- Kubernetes Security Best Practices:
  https://kubernetes.io/docs/concepts/security/

- Kubernetes Admission Controllers:
  https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/