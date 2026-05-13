# Kubernetes Supply Chain Security

This section covers production-grade software supply chain security concepts using:

- Trivy
- Cosign
- Sigstore
- SBOMs
- Attestations
- SLSA
- CI/CD security

These concepts are critical for modern:
- DevSecOps
- Zero Trust Kubernetes
- Container Security
- Enterprise CI/CD pipelines

---

# Directory Structure

```text
05-supply-chain-security/
├── supply-chain-basics.md
├── cosign-installation.md
├── image-signing.md
├── verify-images.md
├── attestations.md
├── sbom.md
├── trusted-registries.md
├── vulnerability-gating.md
├── slsa.md
└── ci-cd-integration.md
```

---

# Very Important Production Lessons

## 1. Signing Without Verification Is Incomplete

Signing images alone is NOT enough.

You must also:
- verify signatures during deployment

---

## 2. Keyless Signing Is Becoming Standard

Benefits:
- no key management
- identity-based trust
- easier rotation

---

## 3. Vulnerability Scanning Must Happen Early

Scan:
- source code
- dependencies
- container images

before deployment.

---

## 4. Registry Security Matters

Use:
- private registries
- immutable tags
- signed artifacts

---

## 5. CI/CD Pipelines Are High-Value Targets

Protect:
- secrets
- runners
- signing workflows
- OIDC permissions

---

## 6. Audit Before Enforce

Always start verification policies with:

```yaml
validationFailureAction: Audit
```

before moving to:
- Enforce

---

# Recommended Learning Order

1. supply-chain-basics
2. cosign-installation
3. image-signing
4. verify-images
5. attestations
6. sbom
7. trusted-registries
8. vulnerability-gating
9. slsa
10. ci-cd-integration

---

# Useful Commands

## Verify Image Signature

```bash
cosign verify harbor.example.com/app:v1
```

---

## Generate SBOM

```bash
syft image:tag -o spdx-json
```

---

## Scan Image

```bash
trivy image image:tag
```

---

## List Kyverno Policies

```bash
kubectl get pol
kubectl get cpol
```

---

# References

- Official Kyverno Documentation:
  https://kyverno.io

- Sigstore:
  https://sigstore.dev

- Cosign:
  https://github.com/sigstore/cosign

- SLSA Framework:
  https://slsa.dev

- Trivy:
  https://aquasecurity.github.io/trivy/

- Syft:
  https://github.com/anchore/syft