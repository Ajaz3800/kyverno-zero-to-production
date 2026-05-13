# 6. sbom

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