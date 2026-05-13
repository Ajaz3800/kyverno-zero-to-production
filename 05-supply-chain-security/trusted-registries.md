# 7. trusted-registries

## Goal

Allow images only from approved registries.

---

# Why Important

Prevents:
- unknown public images
- malicious registries
- supply chain attacks

---

# Example Trusted Registries

- Harbor
- ECR
- GCR
- ACR

---

# Example Kyverno Policy

```yaml
imageReferences:
  - "harbor.example.com/*"
```

---

# Production Recommendation

Use:
- private registries
- signed images
- immutable tags

---