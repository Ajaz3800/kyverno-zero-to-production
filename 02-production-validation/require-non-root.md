# 1. require-non-root

## Goal

Ensure containers do not run as the root user.

## Why Important

Running containers as root increases:
- privilege escalation risks
- container breakout risks
- host compromise risks

## Production Best Practice

Use:

```yaml
securityContext:
  runAsNonRoot: true
```

---