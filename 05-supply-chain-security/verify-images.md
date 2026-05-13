# 4. verify-images

## Goal

Verify signed images during Kubernetes admission.

---

# Verification Flow

```text
Deployment Request
      ↓
Kyverno verifyImages
      ↓
Cosign Signature Validation
      ↓
ALLOW / DENY
```

---

# Example Verification Policy

```yaml
verifyImages:
  - imageReferences:
      - "harbor.example.com/*"
```

---

# Why Important

Prevents:
- unsigned images
- tampered images
- untrusted artifacts

from running in production.

---