# 6. disallow-hostpath

## Goal

Prevent use of `hostPath` volumes.

## Why Important

`hostPath` can expose:
- host filesystem
- Kubernetes node data
- container runtime files

This is considered high risk.

## Production Recommendation

Avoid `hostPath` whenever possible.

---