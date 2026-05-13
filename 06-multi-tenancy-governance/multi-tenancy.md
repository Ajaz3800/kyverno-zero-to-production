# 1. multi-tenancy

## Goal

Understand multi-tenant Kubernetes architecture.

---

# What Is Multi-Tenancy?

Multiple:
- teams
- applications
- business units
- environments

share the same Kubernetes cluster securely.

---

# Core Requirements

- namespace isolation
- RBAC separation
- network isolation
- resource quotas
- policy enforcement

---

# Production Challenges

Without governance:
- tenants affect each other
- noisy neighbor issues occur
- security boundaries weaken

---