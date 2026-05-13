# Kyverno Production Learning Guide

This repository contains my hands-on learning and production-level notes for using :contentReference[oaicite:0]{index=0} in Kubernetes clusters.

---

# Table of Contents

- Kyverno Architecture
- Kyverno Controllers
- Installation
- Validation Policy
- Mutation Policy
- Policy Reports
- Fail-Open vs Fail-Close
- Very Important Production Lessons

---

# Kyverno Architecture

Kyverno works as a Kubernetes-native policy engine.

It integrates directly with the Kubernetes Admission Controller mechanism.

## Architecture Flow

```text
kubectl apply
      ↓
Kubernetes API Server
      ↓
Kyverno Admission Webhook
      ↓
Policy Evaluation
      ↓
ALLOW / DENY / MUTATE
      ↓
Object Stored in etcd
```

## Main Features

- Validation Policies
- Mutation Policies
- Generate Policies
- Cleanup Policies
- Policy Reporting
- Background Scanning

---

# Kyverno Controllers

Kyverno runs multiple controllers inside the cluster.

## 1. Admission Controller

Responsible for:
- validating resources
- mutating resources
- blocking requests

Example:
- Block `latest` image tag
- Add labels automatically

---

## 2. Background Controller

Responsible for:
- scanning existing resources
- generating policy reports

Works when:

```yaml
background: true
```

---

## 3. Reports Controller

Responsible for:
- creating PolicyReports
- storing audit results

Useful for:
- compliance
- security visibility
- dashboards

---

## 4. Cleanup Controller

Responsible for:
- cleanup policies
- deleting old resources automatically

---

# Installation

## Add Helm Repository

```bash
helm repo add kyverno https://kyverno.github.io/kyverno/
helm repo update
```

---

## Install Kyverno

```bash
helm install kyverno kyverno/kyverno -n kyverno --create-namespace
```

---

## Verify Installation

```bash
kubectl get pods -n kyverno
```

Expected Pods:

```text
kyverno-admission-controller
kyverno-background-controller
kyverno-cleanup-controller
kyverno-reports-controller
```

---

# Validation Policy

Validation policies check whether resources follow security/compliance rules.

Example:
- Block `latest` image tags
- Require labels
- Require resource limits

## Example Validation Policy

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-latest-tag

spec:
  validationFailureAction: Enforce
  background: true

  rules:
    - name: require-image-tag

      match:
        any:
          - resources:
              kinds:
                - Pod

      validate:
        message: "Using latest tag is not allowed."

        pattern:
          spec:
            containers:
              - image: "!*:latest"
```

---

# Mutation Policy

Mutation policies automatically modify Kubernetes resources.

Example:
- Add labels
- Add annotations
- Inject security settings
- Add tolerations

## Example Mutation Policy

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: add-label

spec:
  rules:
    - name: add-env-label

      match:
        any:
          - resources:
              kinds:
                - Pod

      mutate:
        patchStrategicMerge:
          metadata:
            labels:
              +(environment): dev
```

## Verify Mutation

```bash
kubectl get pod good-pod --show-labels
```

---

# Policy Reports

Kyverno generates reports for audit violations.

## List Reports

```bash
kubectl get polr -A
```

---

## Describe Report

```bash
kubectl describe polr <report-name> -n default
```

---

## Example Output

```text
Policy: disallow-latest-tag
Rule: require-image-tag
Result: fail
Message: Using latest tag is not allowed.
```

---

# Fail-Open vs Fail-Close

This is a VERY important production concept.

## Fail-Open

If Kyverno webhook fails:
- Kubernetes still allows requests

Pros:
- Better availability

Cons:
- Security policies bypassed

---

## Fail-Close

If Kyverno webhook fails:
- Kubernetes blocks requests

Pros:
- Strong security

Cons:
- Can break deployments during outages

---

## Production Recommendation

- Dev/Test → Fail-Open acceptable
- Production Security Policies → Prefer Fail-Close carefully

---

# VERY IMPORTANT Production Lessons

## 1. Start Policies in Audit Mode

Never enforce immediately.

Use:

```yaml
validationFailureAction: Audit
```

Why?
- Prevent production outages
- Observe violations first
- Safely roll out policies

Production rollout pattern:

```text
Audit → Observe → Fix → Enforce
```

---

## 2. Test in Dev First

Policies can break deployments.

Always:
- test in development
- validate GitOps flows
- verify CI/CD compatibility

before enabling in production.

---

## 3. Mutation Can Affect GitOps

Mutation policies can create drift issues with tools like :contentReference[oaicite:1]{index=1}.

Example:
- Git says one thing
- Kyverno mutates resource
- ArgoCD detects drift

This is a real production challenge.

---

## 4. Observe Admission Latency

Too many policies can slow Kubernetes API requests.

Every admission request passes through:
- validation
- mutation
- webhook processing

Monitor:
- API latency
- webhook performance
- controller resource usage

---

## 5. Scope Policies Carefully

Overly broad policies can cause outages.

Bad Example:
- Applying strict policies to all namespaces immediately

Better Approach:
- start with dev namespaces
- use exclusions
- gradually expand scope

---

# Key Production Best Practices

- Use immutable image tags
- Start with Audit mode
- Monitor PolicyReports
- Test policies in lower environments
- Keep policies scoped and controlled
- Monitor admission webhook latency
- Avoid broad cluster-wide enforcement initially

---

# Useful Commands

## Get Policies

```bash
kubectl get cpol
```

---

## Describe Policy

```bash
kubectl describe cpol <policy-name>
```

---

## Get Policy Reports

```bash
kubectl get polr -A
```

---

## Check Kyverno Pods

```bash
kubectl get pods -n kyverno
```

---

# References

- Official Kyverno Documentation:
  https://kyverno.io

- Kubernetes Admission Controllers:
  https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/