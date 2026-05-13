# 10. optimization

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