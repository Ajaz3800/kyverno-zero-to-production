# 1. supply-chain-basics

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