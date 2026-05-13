# 10. ci-cd-integration

## Goal

Integrate supply chain security into CI/CD.

---

# Secure CI/CD Flow

```text
Developer Push
      ↓
GitHub Actions / GitLab CI
      ↓
Build Container
      ↓
Generate SBOM
      ↓
Trivy Scan
      ↓
Cosign Sign
      ↓
Push To Registry
      ↓
Kyverno Verify
      ↓
Deploy To Kubernetes
```

---

# Common CI/CD Security Steps

## 1. Dependency Scanning

Example:
- npm audit
- pip-audit
- Trivy fs scan

---

## 2. Image Scanning

Example:
- Trivy image scan

---

## 3. SBOM Generation

Example:
- Syft

---

## 4. Image Signing

Example:
- Cosign

---

## 5. Admission Verification

Example:
- Kyverno verifyImages

---

# GitHub Actions Example

```yaml
permissions:
  contents: read
  packages: write
  id-token: write
```

---

# Why id-token: write?

Required for:
- keyless signing
- GitHub OIDC authentication

---