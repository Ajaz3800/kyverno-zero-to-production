# 4. policyreports

## Goal

Understand PolicyReports operationally.

---

# What Are PolicyReports?

PolicyReports store:
- policy violations
- audit results
- compliance data

---

# View Reports

```bash
kubectl get polr -A
```

---

# Describe Report

```bash
kubectl describe polr <report-name> -n <namespace>
```

---

# Common Report Results

| Result | Meaning |
|---|---|
| pass | Policy satisfied |
| fail | Policy violated |
| warn | Warning generated |
| error | Policy execution issue |
| skip | Rule skipped |

---