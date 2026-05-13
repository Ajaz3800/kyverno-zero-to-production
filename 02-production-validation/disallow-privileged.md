# 2. disallow-privileged

## Goal

Block privileged containers.

## Why Important

Privileged containers can:
- access host devices
- bypass namespace isolation
- compromise cluster security

## Production Recommendation

Never allow:

```yaml
privileged: true
```

unless absolutely necessary.

---