# 2. strategic-merge

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