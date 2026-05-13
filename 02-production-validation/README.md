# Production Validation Policies

This section contains production-grade validation policies.

These policies help enforce:
- Kubernetes security best practices
- workload compliance
- DevSecOps standards
- production reliability

---

# Directory Structure

```text
02-production-validation/
├── require-non-root.md
├── disallow-privileged.md
├── require-resources.md
├── require-probes.md
├── restrict-registries.md
├── disallow-hostpath.md
├── require-labels.md
└── rollout-strategy.md
```

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

# Important Production Lessons

## 1. Policies Can Break Production

Always test in:
- dev
- staging
- pre-production

before cluster-wide rollout.

---

## 2. GitOps Drift Can Happen

Mutation policies can create drift with tools like : Argocd.

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