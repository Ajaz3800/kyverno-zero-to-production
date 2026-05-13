# 9. resourcequota-generation

## Goal

Automatically create ResourceQuotas.

## Why Important

Prevents:
- namespace resource exhaustion
- noisy neighbor problems
- uncontrolled CPU/memory usage

---

## Example Resources

```yaml
ResourceQuota
LimitRange
```

---