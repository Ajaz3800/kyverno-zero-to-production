# Advanced Kyverno Concepts

This section covers advanced production-grade concepts.

These topics are critical for:
- enterprise Kubernetes security
- scalable policy management
- DevSecOps automation
- multi-team governance
- production optimization

---

# Directory Structure

```text
04-advanced-concepts/
├── preconditions.md
├── deny-rules.md
├── jmespath.md
├── variables.md
├── anchors.md
├── exceptions.md
├── namespace-selectors.md
├── context.md
├── policy-layering.md
└── optimization.md
```

---

# Very Important Production Lessons

## 1. Overly Broad Policies Are Dangerous

Badly scoped policies can:
- affect entire clusters
- break deployments
- create outages

---

## 2. Exceptions Are Necessary

Perfect compliance is unrealistic.

Controlled exceptions are part of real production operations.

---

## 3. Performance Matters

Kyverno runs inline with:
- Kubernetes admission requests

Poor policy design directly impacts:
- cluster responsiveness

---

## 4. GitOps Compatibility Is Critical

Mutation and layering strategies must work safely with:
- ArgoCD
- FluxCD

---

## 5. Namespace-Based Rollout Is Safer

Start with:
- namespace-scoped policies
- gradual rollout

before cluster-wide enforcement.

---

# Recommended Learning Order

1. variables
2. anchors
3. preconditions
4. deny-rules
5. jmespath
6. namespace-selectors
7. exceptions
8. context
9. policy-layering
10. optimization

---

# Useful Commands

## List Policies

```bash
kubectl get pol
kubectl get cpol
```

---

## Describe Policy

```bash
kubectl describe pol <policy-name> -n <namespace>
```

---

## View Reports

```bash
kubectl get polr -A
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

- JMESPath Documentation:
  https://jmespath.org/

- Kubernetes Admission Controllers:
  https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/