# Beacon

A small SRE command center I'm building on my laptop to practise running a production-like system from end to end: deploying it, watching it, breaking it and fixing it.

## Why I'm building this

In most ops teams I've worked with, the information you need during an incident is spread across too many places. SLOs live in one dashboard, alerts in another tool, the change that caused the problem is somewhere in Git or Jira, and the post-mortem ends up in a document nobody finds again. With NIS2 there is now also a reporting deadline running in the background.

Beacon puts those pieces in one place. It is also how I'm learning every tool in the stack properly — by building it myself instead of only reading about it.

## What it does (planned)

There are two parts.

**VoltFleet** is a simulated fleet of electric cars that send telemetry: speed, battery level, temperature and location. This is the "production" system that Beacon looks after, and the thing I break on purpose.

**Beacon** is the operations side:

- **SLOs and error budgets** — define SLIs per service, follow the error budget, export a monthly SLA report
- **Incidents** — alerts open P1/P2 incidents automatically, with a timeline, status updates and an RCA page
- **Releases** — change record, automated tests, rollout through Argo CD, post-checks and rollback
- **NIS2** — the 24 h / 72 h / 1 month reporting clocks and a simple risk register
- **Chaos drills** — buttons that inject real faults (OOM, bad deploy, full disk, expired certificate, Terraform drift) and measure how long recovery takes
- **AI assistant** — a local model that reads events and logs, suggests the next runbook step and drafts the RCA. Everything keeps working when the model is switched off.

## Tech stack

Everything is free and runs locally.

| Area | Tools |
|---|---|
| Backend | Python, FastAPI, SQLAlchemy, Alembic |
| Frontend | React, TypeScript, Vite, Tailwind CSS, shadcn/ui |
| Data | PostgreSQL (TimescaleDB, pgvector), Redis |
| Platform | Docker, kind, Helm, Argo CD, Terraform |
| Observability | Prometheus, Grafana, Loki, Tempo, OpenTelemetry |
| Testing | pytest, Testcontainers, Playwright, k6 |
| Security | Trivy, Kyverno, Cosign, Syft |
| AI | Ollama, running on the local GPU |
| CI/CD | GitHub Actions, GitHub Container Registry, Argo CD |

## Repository layout

```
apps/web             React frontend
services/api         Beacon API (FastAPI)
services/simulator   VoltFleet telemetry simulator
services/worker      background processing
deploy/helm          Helm charts
deploy/argocd        Argo CD applications (GitOps)
infra/terraform      infrastructure as code
observability/       dashboards, alert rules, SLO definitions
tests/e2e            end-to-end and acceptance tests
docs/adr             architecture decision records
docs/runbooks        incident runbooks
```

## Roadmap

1. Git and repository setup
2. Backend and database
3. Frontend
4. Containers (Docker)
5. Local Kubernetes (kind) and Helm
6. CI pipeline with tests
7. GitOps with Argo CD
8. Observability and SLOs
9. Incidents, NIS2, AI assistant and chaos drills

## Status

🚧 Phase 1 — Git basics done.

## Running it locally

Instructions will follow once the first service exists (Phase 4).