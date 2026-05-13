# Kyverno Mutation & Generation Policies

This section contains production-focused mutation and generation policy.

Mutation policies automatically modify Kubernetes resources during admission.

Generate policies automatically create Kubernetes resources such as:
- NetworkPolicies
- ResourceQuotas
- LimitRanges
- ConfigMaps

These are extremely important for:
- platform engineering
- Kubernetes governance
- DevSecOps automation
- multi-tenant clusters

---

# Directory Structure

```text
03-mutation/
├── mutation-basics.md
├── strategic-merge.md
├── foreach.md
├── auto-labels.md
├── image-pull-policy.md
├── security-context.md
├── generate-rules.md
├── network-policy-generation.md
├── resourcequota-generation.md
└── gitops-safe-mutation.md
```

---

# Mutation vs Validation

| Feature | Validation | Mutation |
|---|---|---|
| Blocks bad configs | ✅ | ❌ |
| Automatically modifies resources | ❌ | ✅ |
| Generates reports | ✅ | Limited |
| Admission-time processing | ✅ | ✅ |

---

# Recommended GitOps-Safe Strategies

## 1. Mutate Only Missing Fields

Use:

```yaml
+(field)
```

Example:

```yaml
+(environment): production
```

---

## 2. Avoid Aggressive Mutation

Avoid mutating:
- replicas
- selectors
- immutable fields

---

## 3. Scope Policies Carefully

Start with:
- dev namespaces
- non-critical workloads

---

## 4. Test With GitOps Controllers

Always validate:
- ArgoCD sync behavior
- reconciliation loops
- drift detection

before production rollout.

---

# Mutation Processing Flow

```text
kubectl apply
      ↓
API Server
      ↓
Kyverno Mutating Webhook
      ↓
Resource Modified
      ↓
Stored in etcd
```

---

# Important Production Lessons

## 1. Mutation Happens During Admission

Kyverno mutates:
- CREATE
- UPDATE

requests only.

It does NOT continuously mutate running resources.

---

## 2. Existing Resources Are Not Automatically Modified

Mutation applies only to:
- new resources
- updated resources

unless explicitly re-triggered.

---

## 3. Mutation Can Increase Admission Latency

Too many mutation rules can:
- slow API requests
- affect deployment speed

Monitor:
- webhook latency
- API server performance

---

## 4. Generate Rules Can Create Large Blast Radius

Badly scoped generate rules may:
- create unwanted resources cluster-wide
- affect many namespaces

Always test carefully.

---

## 5. Namespace-Based Rollout Is Safer

Prefer:

```yaml
kind: Policy
```

before using:

```yaml
kind: ClusterPolicy
```

for mutation rollouts.

---

# Recommended Learning Order

1. mutation-basics
2. strategic-merge
3. foreach
4. auto-labels
5. image-pull-policy
6. security-context
7. generate-rules
8. network-policy-generation
9. resourcequota-generation
10. gitops-safe-mutation

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

## Check Kyverno Controllers

```bash
kubectl get pods -n kyverno
```

---

## View Policy Reports

```bash
kubectl get polr -A
```

---

# References

- Official Kyverno Documentation:
  https://kyverno.io

- Kubernetes Admission Controllers:
  https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/

- ArgoCD Documentation:
  https://argo-cd.readthedocs.io/