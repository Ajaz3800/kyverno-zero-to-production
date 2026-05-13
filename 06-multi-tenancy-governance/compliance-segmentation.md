# 8. compliance-segmentation

## Goal

Apply policies based on compliance requirements.

---

# Example Compliance Domains

- PCI-DSS
- HIPAA
- SOC2
- ISO27001

---

# Example Namespace Labels

```yaml
compliance: pci
```

---

# Why Important

Different workloads require:
- different security baselines
- different controls
- different audit requirements

---

# Production Strategy

Use:
- namespace labels
- namespace selectors
- policy layering

for scalable compliance enforcement.

---