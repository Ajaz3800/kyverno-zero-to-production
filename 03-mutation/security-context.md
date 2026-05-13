# 6. security-context

## Goal

Automatically enforce secure container settings.

## Common Security Settings

```yaml
runAsNonRoot: true
readOnlyRootFilesystem: true
allowPrivilegeEscalation: false
```

---

## Why Important

Reduces:
- privilege escalation
- container breakout risks
- insecure workloads

---