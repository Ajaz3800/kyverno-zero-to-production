# 2. namespace-governance

## Goal

Standardize namespace security and governance.

---

# Namespace Governance Includes

- labels
- quotas
- network policies
- RBAC
- Pod Security
- Kyverno policies

---

# Example Namespace Labels

```yaml
labels:
  team: payments
  environment: production
  compliance: pci
```

---

# Why Important

Namespace labels enable:
- policy targeting
- compliance segmentation
- tenant management

---