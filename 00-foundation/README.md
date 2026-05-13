# Kubernetes API Request Flow

### Every Kubernetes action goes through the API Server.

Example:
```bash
kubectl apply -f pod.yaml
```
The request flow:
```
User
  ↓
kubectl
  ↓
Kubernetes API Server
  ↓
Authentication
  ↓
Authorization (RBAC)
  ↓
Admission Controllers
  ↓
ETCD
  ↓
Scheduler/Kubelet
```
Kyverno operates here:
```
Admission Controllers
       ↑
    Kyverno
```
This is the CORE foundation.

# What are the Admission Controllers?
Admission controllers are plugins that:
- inspect requests,
- modify requests,
- allow/deny requests.

Examples:
- deny privileged pods
- add labels automatically
- inject sidecars
- block insecure images
Kyverno is an admission controller.

# Two Types of Admission Webhooks

1. Mutating Admission Webhook

Can MODIFY requests before creation.

Example:
- add labels
- add annotations
- inject securityContext
- set default values

Example flow:
```
User creates Pod
        ↓
Kyverno MUTATES Pod
        ↓
Modified Pod stored
```
2. Validating Admission Webhook

Can ALLOW or DENY requests.

Example:
- deny privileged container
- require probes
- require limits
- block latest tag

Example:
```
User creates insecure Pod
        ↓
Kyverno VALIDATES
        ↓
DENIED
```
# IMPORTANT ORDER
Kubernetes processes requests like this:
```
Mutating Webhooks
       ↓
Object Updated
       ↓
Validating Webhooks
       ↓
Final Decision
```
Meaning:

- mutation happens FIRST
- validation happens AFTER

This is EXTREMELY important in production.

# Real Example
Suppose developers forget labels.
Mutation policy:
```yaml
add:
  labels:
    environment: dev
```
Validation policy:
```yaml
must have environment label
```
Flow:
```
Pod Created
  ↓
Mutation adds label
  ↓
Validation passes
```
Without understanding order, policies break.

# Kubernetes Security Concepts
Kyverno exists because Kubernetes is insecure by default.

You must understand common risks.

# Dangerous Pod Configurations
Privileged Containers

VERY dangerous:
```yaml
  securityContext:
    privileged: true
```
Why dangerous?

- container gets host-level access
- container escape possible

Production policy:
```
DENY privileged containers
```
# Running as Root

Bad:
```
runAsUser: 0
```
Risk:

- root access inside container
- privilege escalation

Production:
```
Require non-root containers
```
# hostPath Volumes

Dangerous:
```yaml
hostPath:
  path: /
```
Risk:

- container accesses host filesystem

Production:
```
Block hostPath
```
# Latest Tag

Bad:
```yaml
image: nginx:latest
```
Risk:

- non-repeatable deployments
- supply-chain instability

Production:
```
Deny latest tag
```
# Missing Resource Limits

Bad:
```yaml
resources: {}
```
Risk:

- noisy neighbor problems
- cluster instability

Production:
```
Require requests and limits
```

# Pod Security Standards (PSS)

Kubernetes replaced PodSecurityPolicy with:

- Pod Security Admission
- Pod Security Standards

Three levels:

1. Privileged

Almost unrestricted.

Use cases:

- system workloads
- infrastructure components

NOT for apps.

2. Baseline

Basic security protections.

Blocks:

- privileged containers
- some host access

Good starting point.

3. Restricted

Strong production security.

Requires:

- non-root
- read-only filesystem
- dropped capabilities
- seccomp
- no privilege escalation

Most enterprises aim here.

# Why Kyverno If PSS Exists?

PSS is limited.

Kyverno provides:

- custom policies
- mutation
- image verification
- generate rules
- exceptions
- GitOps integrations
- advanced logic

PSS = basic controls
Kyverno = enterprise governance

# GitOps Impact

This is VERY important.

## Problem Example

ArgoCD deploys:
```yaml
replicas: 2
```
Kyverno mutates:
```yaml
replicas: 3
```
ArgoCD sees drift:
```
Desired != Actual
```
Result:

- sync loops
- constant reconciliation
- unstable deployments

# Production Lesson

Mutation policies must be GitOps-aware.

We’ll learn:

- safe mutation
- sync exclusions
- compare options
- mutation strategies

later in production phases.