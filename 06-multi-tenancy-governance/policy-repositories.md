# 6. policy-repositories

## Goal

Manage policies using dedicated repositories.

---

# Recommended Structure

```text
policies/
├── validation/
├── mutation/
├── generate/
├── exceptions/
└── environments/
```

---

# Why Important

Dedicated repositories improve:
- policy versioning
- code reviews
- auditability
- team collaboration

---

# Production Recommendation

Treat policies like:
- production application code

with:
- pull requests
- CI validation
- approvals

---