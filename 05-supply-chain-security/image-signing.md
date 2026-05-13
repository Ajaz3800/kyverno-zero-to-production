# 3. image-signing

## Goal

Sign container images securely.

---

# Signing Types

| Type | Description |
|---|---|
| Static Key Signing | Uses public/private keys |
| Keyless Signing | Uses OIDC identity |

---

# Static Key Example

```bash
cosign generate-key-pair

cosign sign --key cosign.key harbor.example.com/app:v1
```

---

# Keyless Signing Example

```bash
COSIGN_EXPERIMENTAL=true cosign sign --yes harbor.example.com/app:v1
```

---

# Production Recommendation

Prefer:
- keyless signing
- OIDC identity verification

over long-lived static keys.

---