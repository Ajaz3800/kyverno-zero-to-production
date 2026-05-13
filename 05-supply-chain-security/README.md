# Kubernetes Supply Chain Security

This section covers production-grade software supply chain security concepts using:

- :contentReference[oaicite:0]{index=0}
- :contentReference[oaicite:1]{index=1}
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

# 1. supply-chain-basics.md

## Goal

Understand software supply chain security fundamentals.

---

# What Is Supply Chain Security?

Supply chain security protects:
- source code
- build systems
- container images
- dependencies
- deployment pipelines

from tampering and compromise.

---

# Modern Threats

Examples:
- malicious dependencies
- compromised CI/CD
- tampered images
- registry poisoning
- stolen signing keys

---

# Secure Supply Chain Flow

```text
Developer Push
      ↓
CI/CD Pipeline
      ↓
Build Image
      ↓
Generate SBOM
      ↓
Scan Vulnerabilities
      ↓
Sign Image
      ↓
Push To Registry
      ↓
Verify During Deployment
```

---

# 2. cosign-installation.md

## Goal

Install and configure Cosign.

---

# What Is Cosign?

:contentReference[oaicite:2]{index=2} is a container signing tool from Sigstore.

It helps:
- sign images
- verify signatures
- generate attestations
- secure container supply chains

---

# Install Cosign

## Linux

```bash
curl -O -L https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64

chmod +x cosign-linux-amd64

sudo mv cosign-linux-amd64 /usr/local/bin/cosign
```

---

# Verify Installation

```bash
cosign version
```

---

# 3. image-signing.md

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

# 4. verify-images.md

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

# 5. attestations.md

## Goal

Understand signed attestations.

---

# What Are Attestations?

Attestations are signed metadata attached to images.

Examples:
- vulnerability scan results
- SBOMs
- build provenance
- test results

---

# Why Important

Attestations help verify:
- how image was built
- who built it
- what security checks passed

---

# Example Use Cases

- Verify vulnerability scans passed
- Require trusted CI/CD pipeline
- Enforce build provenance

---

# 6. sbom.md

## Goal

Generate and validate SBOMs.

---

# What Is SBOM?

SBOM = Software Bill of Materials

An SBOM lists:
- packages
- dependencies
- libraries
- versions

inside an image.

---

# Why Important

Helps with:
- vulnerability management
- compliance
- dependency tracking
- incident response

---

# Example Tools

- Syft
- Trivy
- CycloneDX
- SPDX

---

# Example SBOM Generation

```bash
syft harbor.example.com/app:v1 -o spdx-json > sbom.json
```

---

# 7. trusted-registries.md

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

# 8. vulnerability-gating.md

## Goal

Block vulnerable images from deployment.

---

# Workflow

```text
Build Image
      ↓
Trivy Scan
      ↓
Critical Vulnerability Found?
      ↓
BLOCK Deployment
```

---

# Why Important

Prevents vulnerable workloads from reaching production.

---

# Common Tools

- Trivy
- Grype
- Clair

---

# Production Best Practice

Block:
- Critical vulnerabilities
- High vulnerabilities

before deployment.

---

# 9. slsa.md

## Goal

Understand SLSA framework basics.

---

# What Is SLSA?

SLSA = Supply-chain Levels for Software Artifacts

SLSA defines maturity levels for:
- build integrity
- provenance
- CI/CD security

---

# SLSA Goals

- prevent tampering
- secure builds
- improve provenance
- enforce reproducibility

---

# SLSA Levels

| Level | Focus |
|---|---|
| SLSA 1 | Build tracking |
| SLSA 2 | Hosted build service |
| SLSA 3 | Hardened build platform |
| SLSA 4 | Reproducible builds |

---

# Production Importance

Modern enterprises increasingly require:
- SLSA compliance
- signed provenance
- hardened CI/CD

---

# 10. ci-cd-integration.md

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