# 9. tenant-isolation

## Goal

Securely isolate tenants inside clusters.

---

# Isolation Components

- namespaces
- RBAC
- NetworkPolicies
- Pod Security
- ResourceQuotas
- Kyverno policies

---

# Production Risks Without Isolation

- lateral movement
- resource exhaustion
- accidental cross-team access

---

# Recommended Isolation Model

```text
Tenant
   ↓
Namespace
   ↓
RBAC
   ↓
NetworkPolicy
   ↓
Policy Enforcement
```

---