# Advanced Kyverno Concepts

This section covers advanced production-grade concepts in :contentReference[oaicite:0]{index=0}.

These topics are critical for:
- enterprise Kubernetes security
- scalable policy management
- DevSecOps automation
- multi-team governance
- production optimization

---

# 1. preconditions.md

## Goal

Apply rules only when specific conditions match.

---

# Why Important

Preconditions help:
- avoid unnecessary policy execution
- reduce false positives
- scope policies dynamically

---

# Example

```yaml
preconditions:
  all:
    - key: "{{ request.operation }}"
      operator: Equals
      value: CREATE
```

---

# Common Use Cases

- Only validate CREATE requests
- Skip DELETE operations
- Target specific labels
- Apply rules only for certain users

---

# Production Benefit

Preconditions improve:
- performance
- policy flexibility
- operational safety

---

# 2. deny-rules.md

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

# 3. jmespath.md

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

# 4. variables.md

## Goal

Use dynamic variables inside policies.

---

# Common Variables

```yaml
{{ request.object.metadata.name }}
{{ request.namespace }}
{{ request.userInfo.username }}
```

---

# Why Important

Variables help create:
- reusable policies
- dynamic validation
- user-aware rules

---

# Common Use Cases

- Namespace-aware policies
- User-based restrictions
- Dynamic mutation
- Audit metadata injection

---

# 5. anchors.md

## Goal

Control matching and mutation behavior precisely.

---

# Common Anchors

| Anchor | Purpose |
|---|---|
| `()` | Conditional anchor |
| `=` | Equality anchor |
| `^()` | Existence anchor |
| `+()` | Add-if-not-present |

---

# Example

```yaml
+(environment): production
```

---

# Why Important

Anchors improve:
- safe mutation
- GitOps compatibility
- conditional matching

---

# Production Recommendation

Prefer:

```yaml
+()
```

for GitOps-safe mutation.

---

# 6. exceptions.md

## Goal

Exclude workloads from policy enforcement safely.

---

# Why Important

Production clusters often require exemptions for:
- platform workloads
- monitoring systems
- ingress controllers
- legacy applications

---

# Example

```yaml
exclude:
  any:
    - resources:
        namespaces:
          - kube-system
          - monitoring
```

---

# Production Best Practice

Always document:
- who requested exception
- why exception exists
- expiration timeline

---

# 7. namespace-selectors.md

## Goal

Apply policies dynamically using namespace labels.

---

# Example

```yaml
namespaceSelector:
  matchLabels:
    environment: production
```

---

# Why Important

Namespace selectors enable:
- scalable governance
- multi-tenant policy management
- environment-based enforcement

---

# Production Benefit

Avoid hardcoding namespace names.

Prefer:
- labels
- selectors
- dynamic grouping

---

# 8. context.md

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

# 9. policy-layering.md

## Goal

Design scalable multi-layer policy architectures.

---

# Recommended Layering Model

```text
Cluster Baseline Policies
        ↓
Environment Policies
        ↓
Namespace Policies
        ↓
Team-Specific Policies
```

---

# Why Important

Policy layering improves:
- scalability
- governance
- operational control

---

# Example Strategy

| Layer | Purpose |
|---|---|
| ClusterPolicy | Global baseline security |
| Namespace Policy | Team/environment customization |
| Exceptions | Controlled overrides |

---

# Production Recommendation

Avoid:
- giant monolithic policies

Prefer:
- modular layered policies

---

# 10. optimization.md

## Goal

Improve Kyverno performance and scalability.

---

# Why Important

Poorly designed policies can:
- slow API requests
- increase admission latency
- overload controllers

---

# Optimization Best Practices

## 1. Scope Policies Carefully

Avoid:
- cluster-wide broad matching

Prefer:
- namespace scoping
- selectors

---

## 2. Use Preconditions

Prevent unnecessary rule execution.

---

## 3. Minimize External Context Calls

API calls increase:
- latency
- failure risk

---

## 4. Avoid Expensive JMESPath Queries

Complex array processing affects performance.

---

## 5. Use Audit Before Enforce

Reduces:
- outages
- operational surprises

---

# Production Scaling Concerns

Large clusters may process:
- thousands of admission requests per second

Policy efficiency becomes critical.

---

# Advanced Admission Flow

```text
kubectl apply
      ↓
API Server
      ↓
Kyverno Webhook
      ↓
Preconditions
      ↓
Context Loading
      ↓
JMESPath Evaluation
      ↓
Validation / Mutation
      ↓
ALLOW / DENY
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
- :contentReference[oaicite:1]{index=1}
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