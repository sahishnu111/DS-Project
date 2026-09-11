# Microservices Monitoring Dashboard (LGTM + OpenTelemetry Demo)

Production-ready observability project using:
- **Target system:** OpenTelemetry Astronomy Shop demo (`ghcr.io/open-telemetry/demo:latest`)
- **Backend:** Grafana LGTM all-in-one (`grafana/otel-lgtm:latest`)
- **Orchestration:** Docker Compose only

## Quickstart

```bash
cp .env.example .env
docker compose up -d
```

## Prerequisites

- Docker Desktop (or Docker Engine + Compose v2)
- Minimum **8GB RAM** available for containers

## Access URLs

- Grafana: `http://localhost:${GRAFANA_PORT:-3000}` (default: `admin` / `admin`)
- Prometheus: `http://localhost:${PROMETHEUS_PORT:-9090}`
- Loki API: `http://localhost:${LOKI_PORT:-3100}`
- Tempo API: `http://localhost:${TEMPO_PORT:-3200}`
- OpenTelemetry Demo UI: `http://localhost:${DEMO_UI_PORT:-8080}`

## Project Structure

- `docker-compose.yml` - runtime stack and service wiring
- `grafana/provisioning/` - provisioning as code (datasources, dashboards, alerting)
- `grafana/dashboards/golden-signals.json` - golden signals dashboard
- `alerting/` - source-of-truth alert definitions and contact point templates
- `docs/ARCHITECTURE.md` - architecture and team ownership
- `docs/RUNBOOK.md` - incident response and operations

## 4-Week Timeline (30-Day Plan)

### Week 1 - Foundations
- Stand up stack with compose and persistence
- Validate telemetry ingestion (metrics/logs/traces)
- Baseline service inventory and SLIs

### Week 2 - Dashboarding & Correlation
- Build/validate Golden Signals dashboard
- Validate trace-to-logs and trace-to-metrics pivots
- Add annotation conventions for deploy events

### Week 3 - Alerting & Runbooks
- Implement and tune core alerts (error, latency, uptime)
- Configure Slack routing policy
- Finalize incident runbook and drill process

### Week 4 - Hardening & Handover
- Load/chaos checks and alert-noise reduction
- Documentation and onboarding walkthrough
- Final acceptance and operational handoff

## Contributing

1. Create a feature branch from the active PR branch.
2. Keep changes declarative and configuration-as-code.
3. Validate config before commit:
   - `docker compose config`
   - YAML/JSON syntax checks
4. Never commit secrets; use environment variables and `.env`.
5. Open or update PR with focused, reviewable commits.