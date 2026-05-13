# 12. alerting

## Goal

Create operational alerts.

---

# Recommended Alerts

| Alert | Purpose |
|---|---|
| High Admission Latency | Detect performance issues |
| Controller Restarting | Detect instability |
| Webhook Failures | Detect admission problems |
| Policy Violations Spike | Detect security issues |

---

# Recommended Stack

- Prometheus
- Alertmanager
- Slack integration

---

# Production Tip

Avoid noisy alerts.

Focus on:
- actionable signals

---