# Kyverno Mutation & Generation Policies

This section contains production-focused mutation and generation policy examples using :contentReference[oaicite:0]{index=0}.

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

# Mutation vs Validation

| Feature | Validation | Mutation |
|---|---|---|
| Blocks bad configs | ✅ | ❌ |
| Automatically modifies resources | ❌ | ✅ |
| Generates reports | ✅ | Limited |
| Admission-time processing | ✅ | ✅ |

---

# 1. mutation-basics.md

## Goal

Learn basic mutation concepts.

## Mutation Use Cases

- Add labels
- Add annotations
- Inject security settings
- Set imagePullPolicy
- Add tolerations
- Add sidecars

---

## Example

```yaml
mutate:
  patchStrategicMerge:
```

---

# 2. strategic-merge.md

## Goal

Understand Strategic Merge Patch behavior.

## Why Important

Kyverno mutation commonly uses:

```yaml
patchStrategicMerge:
```

This safely merges:
- labels
- annotations
- security settings
- container fields

without replacing the full object.

---

## Example

```yaml
metadata:
  labels:
    +(environment): production
```

---

# 3. foreach.md

## Goal

Mutate or validate all containers dynamically.

## Why Important

Production Pods often contain:
- multiple containers
- initContainers
- sidecars

`foreach` helps process them safely.

---

## Example

```yaml
foreach:
  - list: "request.object.spec.containers[]"
```

---

# 4. auto-labels.md

## Goal

Automatically apply labels.

## Common Labels

```yaml
environment:
team:
owner:
cost-center:
```

---

## Why Important

Labels help with:
- observability
- governance
- cost allocation
- automation
- monitoring

---

# 5. image-pull-policy.md

## Goal

Automatically enforce imagePullPolicy.

## Example

```yaml
imagePullPolicy: Always
```

---

## Why Important

Helps ensure:
- latest images pulled
- stale image prevention
- deployment consistency

---

# 6. security-context.md

## Goal

Automatically enforce secure container settings.

## Common Security Settings

```yaml
runAsNonRoot: true
readOnlyRootFilesystem: true
allowPrivilegeEscalation: false
```

---

## Why Important

Reduces:
- privilege escalation
- container breakout risks
- insecure workloads

---

# 7. generate-rules.md

## Goal

Automatically generate Kubernetes resources.

## Common Generate Use Cases

- NetworkPolicies
- ResourceQuotas
- LimitRanges
- Secrets
- ConfigMaps

---

# Generate Flow

```text
Namespace Created
      ↓
Kyverno Generate Rule
      ↓
Security Resources Created Automatically
```

---

# 8. network-policy-generation.md

## Goal

Automatically create NetworkPolicies.

## Why Important

Prevents namespaces from:
- running without network isolation
- allowing unrestricted traffic

---

## Production Benefit

Every new namespace automatically gets:
- default deny policies
- baseline security

---

# 9. resourcequota-generation.md

## Goal

Automatically create ResourceQuotas.

## Why Important

Prevents:
- namespace resource exhaustion
- noisy neighbor problems
- uncontrolled CPU/memory usage

---

## Example Resources

```yaml
ResourceQuota
LimitRange
```

---

# 10. gitops-safe-mutation.md

## Goal

Understand GitOps-safe mutation strategies.

---

# VERY Important Production Topic

Mutation can conflict with tools like:

- :contentReference[oaicite:1]{index=1}
- FluxCD

Because:
- Git desired state
- Cluster mutated state

may differ.

This creates:
- drift detection issues
- continuous reconciliation loops

---

# Example Drift Scenario

```text
Git Manifest
     ↓
No Label Present
     ↓
Kyverno Adds Label
     ↓
ArgoCD Detects Drift
```

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