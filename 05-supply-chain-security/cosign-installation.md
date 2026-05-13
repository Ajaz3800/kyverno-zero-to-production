# 2. cosign-installation

## Goal

Install and configure Cosign.

---

# What Is Cosign?

Cosign is a container signing tool from Sigstore.

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