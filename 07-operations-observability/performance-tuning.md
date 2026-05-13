# 8. performance-tuning

## Goal

Optimize Kyverno performance.

---

# Optimization Strategies

## 1. Scope Policies Carefully

Avoid:
- unnecessary cluster-wide matching

---

## 2. Use Preconditions

Prevent unnecessary rule execution.

---

## 3. Reduce External Context Calls

API lookups increase latency.

---

## 4. Avoid Expensive JMESPath

Complex queries reduce throughput.

---

## 5. Monitor Controller Resources

Watch:
- CPU
- memory
- restarts

---

# Production Recommendation

Scale Kyverno according to:
- admission request volume
- cluster size
- policy complexity

---