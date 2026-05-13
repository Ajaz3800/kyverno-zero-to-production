# 8, rollout-strategy

## Goal

Safely introduce policies into production.

---

# Recommended Rollout Pattern

```text
Audit
  ↓
Observe Violations
  ↓
Fix Workloads
  ↓
Gradual Namespace Rollout
  ↓
Enforce
```

---

# NEVER Start With Enforce

Bad Approach:

```yaml
validationFailureAction: Enforce
```

on day one.

This can:
- break deployments
- block CI/CD
- cause outages

---

# Recommended Approach

Start with:

```yaml
validationFailureAction: Audit
```

Observe:
- policy reports
- violations
- affected teams

Then gradually move to:
- Enforce

---