# Production Kyverno Learning Repository

Production-grade learning repository for :contentReference[oaicite:0]{index=0} focused on:

- Kubernetes policy management
- admission controllers
- validation & mutation
- supply chain security
- GitOps integration
- multi-tenancy
- production operations
- DevSecOps governance

This repository is designed for:
- DevOps Engineers
- Platform Engineers
- DevSecOps Engineers
- Kubernetes Administrators
- Cloud Security Engineers

---

# Learning Goals

By completing this repository, you will learn:

- Kyverno architecture
- Kubernetes admission control
- validation policies
- mutation policies
- generate rules
- policy reports
- policy optimization
- GitOps-safe governance
- supply chain security
- image verification
- multi-tenant governance
- production operations
- monitoring & troubleshooting

---

# Repository Structure

```text
.
├── 01-core-concepts/
├── 02-production-validation/
├── 03-mutation/
├── 04-advanced-concepts/
├── 05-supply-chain-security/
├── 06-governance-and-multi-tenancy/
├── 07-operations-and-monitoring/
└── README.md
```

---

# Learning Path

## 1. Core Concepts

Understand:
- Kyverno architecture
- controllers
- installation
- validation basics
- mutation basics
- policy reports
- fail-open vs fail-close

```text
01-core-concepts/
```

---

## 2. Production Validation

Learn production-ready validation policies:

- require-non-root
- disallow-privileged
- require-resources
- require-probes
- restrict-registries
- require-labels

```text
02-production-validation/
```

---

## 3. Mutation Policies

Learn:
- strategic merge
- foreach
- auto-labels
- generate rules
- GitOps-safe mutation

```text
03-mutation/
```

---

## 4. Advanced Concepts

Master:
- preconditions
- deny rules
- JMESPath
- variables
- anchors
- exceptions
- context
- optimization

```text
04-advanced-concepts/
```

---

## 5. Supply Chain Security

Learn:
- Cosign
- image signing
- verifyImages
- attestations
- SBOM
- SLSA
- CI/CD integration

```text
05-supply-chain-security/
```

---

## 6. Governance & Multi-Tenancy

Understand:
- namespace governance
- GitOps integration
- ArgoCD drift
- policy promotion
- compliance segmentation
- tenant isolation

```text
06-governance-and-multi-tenancy/
```

---

## 7. Operations & Monitoring

Learn production operations:
- Prometheus integration
- Grafana dashboards
- troubleshooting
- admission latency
- HA deployment
- disaster recovery
- alerting

```text
07-operations-and-monitoring/
```

---

# What Is Kyverno?

:contentReference[oaicite:1]{index=1} is a Kubernetes-native policy engine used to:

- validate resources
- mutate resources
- generate resources
- verify container images
- enforce security standards

Kyverno works through:
- Kubernetes admission controllers
- webhook-based policy enforcement

---

# Why Kyverno Matters

Modern Kubernetes environments require:
- security enforcement
- compliance governance
- workload standardization
- supply chain protection
- multi-tenant isolation

Kyverno enables these capabilities using:
- Kubernetes-native YAML
- declarative policies
- GitOps workflows

---

# Core Kyverno Features

| Feature | Description |
|---|---|
| Validation | Block insecure resources |
| Mutation | Automatically modify resources |
| Generate | Create resources automatically |
| VerifyImages | Verify signed container images |
| PolicyReports | Compliance reporting |
| Exceptions | Controlled policy bypass |

---

# Production Learning Philosophy

This repository focuses heavily on:
- real production practices
- operational safety
- GitOps compatibility
- enterprise governance
- scalability
- troubleshooting

NOT only basic demos.

---

# Recommended Learning Order

```text
Core Concepts
      ↓
Validation Policies
      ↓
Mutation Policies
      ↓
Advanced Concepts
      ↓
Supply Chain Security
      ↓
Governance & Multi-Tenancy
      ↓
Operations & Monitoring
```

---

# Important Production Lessons

## 1. Start In Audit Mode

Always begin with:

```yaml
validationFailureAction: Audit
```

before moving to:
- Enforce

---

## 2. Test In Lower Environments First

Policies can:
- break deployments
- block workloads
- affect GitOps

Always test in:
- dev
- staging

first.

---

## 3. Mutation Can Affect GitOps

Mutation policies may create:
- ArgoCD drift
- reconciliation loops

Use:
- additive mutation
- scoped policies

carefully.

---

## 4. Monitor Admission Latency

Poorly designed policies can:
- slow API requests
- increase deployment times

Monitor:
- webhook latency
- controller performance

---

## 5. Scope Policies Carefully

Avoid overly broad:
- cluster-wide enforcement
- unrestricted matching

Use:
- namespace selectors
- labels
- scoped policies

---

## 6. Policies Are Production Infrastructure

Kyverno directly affects:
- deployments
- admission requests
- cluster operations

Treat policies like:
- production application code

with:
- code review
- CI validation
- promotion pipelines

---

# Recommended Production Stack

| Component | Purpose |
|---|---|
| :contentReference[oaicite:2]{index=2} | Policy enforcement |
| :contentReference[oaicite:3]{index=3} | GitOps delivery |
| :contentReference[oaicite:4]{index=4} | Metrics collection |
| :contentReference[oaicite:5]{index=5} | Dashboards |
| :contentReference[oaicite:6]{index=6} | Image signing |
| Trivy | Vulnerability scanning |
| Syft | SBOM generation |

---

# Example Production Architecture

```text
Developer Push
      ↓
CI/CD Pipeline
      ↓
Build Container
      ↓
Security Scan
      ↓
Generate SBOM
      ↓
Cosign Sign
      ↓
Push To Registry
      ↓
ArgoCD Sync
      ↓
Kyverno Admission Policies
      ↓
Kubernetes Cluster
```

---

# Who Should Use This Repository?

This repository is useful for:

- DevOps Engineers
- DevSecOps Engineers
- Platform Engineers
- Kubernetes Administrators
- Cloud Engineers
- Security Engineers
- SREs

---

# Repository Goals

The goal is to build:
- production-ready understanding
- operational confidence
- real-world Kubernetes governance skills

instead of only theoretical knowledge.

---

# Recommended Labs

Practice:
- validation policies
- mutation policies
- generate rules
- image verification
- policy reports
- GitOps integration
- monitoring setup
- HA deployment
- policy optimization

inside:
- local clusters
- staging environments
- GitOps labs

---

# Recommended Tools

| Tool | Purpose |
|---|---|
| kind | Local Kubernetes |
| minikube | Local Kubernetes |
| kubectl | Kubernetes CLI |
| Helm | Package management |
| Kustomize | Environment overlays |
| GitHub Actions | CI/CD |
| Harbor | Private registry |

---

# Useful Commands

## List Policies

```bash
kubectl get pol
kubectl get cpol
```

---

## View Policy Reports

```bash
kubectl get polr -A
```

---

## Check Kyverno Pods

```bash
kubectl get pods -n kyverno
```

---

## View Kyverno Logs

```bash
kubectl logs -n kyverno deploy/kyverno-admission-controller
```

---

## Describe Policy

```bash
kubectl describe cpol <policy-name>
```

---

# Recommended Next Steps

1. Install Kyverno
2. Learn validation policies
3. Learn mutation policies
4. Understand GitOps impact
5. Implement supply chain security
6. Add monitoring & alerting
7. Practice production troubleshooting

---

# References

- Official Kyverno Documentation:
  https://kyverno.io

- Kubernetes Documentation:
  https://kubernetes.io/docs/

- ArgoCD Documentation:
  https://argo-cd.readthedocs.io/

- Sigstore:
  https://sigstore.dev

- Prometheus:
  https://prometheus.io

- Grafana:
  https://grafana.com