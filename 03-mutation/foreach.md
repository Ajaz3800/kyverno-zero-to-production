# 3. foreach

## Goal

Mutate or validate all containers dynamically.

## Why Important

Production Pods often contain:
- multiple containers
- initContainers
- sidecars

`foreach` helps process them safely.

---

## Example

```yaml
foreach:
  - list: "request.object.spec.containers[]"
```

---