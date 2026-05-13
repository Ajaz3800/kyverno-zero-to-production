# 3. require-resources

## Goal

Require CPU and memory requests/limits.

## Why Important

Without resource limits:
- noisy neighbor problems occur
- scheduling becomes unreliable
- nodes may become unstable

## Production Best Practice

Always define:

```yaml
resources:
  requests:
  limits:
```

---