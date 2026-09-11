# Monitoring Runbook

## Incident Response Procedure

### 1) Alert
- Receive alert in Slack (`slack-default` contact point).
- Identify alert type (`HighErrorRate`, `HighLatencyP95`, `ServiceDown`) and impacted service.

### 2) Dashboard
- Open Grafana Golden Signals dashboard.
- Filter `service_name` variable to impacted service.
- Confirm whether rate, errors, latency, or saturation is breaching SLO.

### 3) Trace
- From latency/error panel datapoints, open exemplar-linked trace in Tempo.
- Inspect span timeline, error attributes, and downstream dependencies.

### 4) Log
- Use Tempo trace-to-logs correlation to pivot into Loki.
- Narrow by service and trace identifiers to isolate failing request path.

## Troubleshooting Guide

### Demo UI not reachable
- Check container health: `docker compose ps`
- Validate UI mapping is bound to `${DEMO_UI_PORT}`.
- Inspect logs: `docker compose logs otel-demo --tail=200`

### No telemetry appearing in Grafana
- Confirm OTLP endpoint/protocol in `otel-demo` env:
  - `OTEL_EXPORTER_OTLP_ENDPOINT=http://grafana-lgtm:4318`
  - `OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf`
- Check LGTM receiver logs: `docker compose logs grafana-lgtm --tail=200`

### Datasources show unhealthy
- Verify provisioning mounts and file paths under `/etc/grafana/provisioning`.
- Confirm internal datasource URLs target LGTM localhost ports (9090/3100/3200).

### Alerts not notifying Slack
- Ensure `SLACK_WEBHOOK_URL` is set in `.env`.
- Validate contact point + policy loaded in Grafana Alerting UI.

## How to Add New Dashboards

1. Add dashboard JSON under `grafana/dashboards/`.
2. Keep provider path unchanged (`/etc/grafana/dashboards`).
3. Restart Grafana container or wait for provisioning refresh interval.

## How to Add New Alerts

1. Add rule definitions to `alerting/rules.yaml`.
2. Follow existing naming, labels, and runbook annotation conventions.
3. Reload Grafana (container restart) and validate rule state in Alerting UI.
