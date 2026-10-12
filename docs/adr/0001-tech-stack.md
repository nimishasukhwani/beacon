# ADR 0001: Tech stack for Beacon

- **Status:** Accepted
- **Date:** 2026-10-12

## Context

Beacon has to run completely on one laptop (16 GB RAM, RTX 4060) and cost nothing.
It should use the same kind of tools a cloud operations team uses in production,
so that what I practise here carries over directly to real work.
I'm the only developer, so the stack should be something I can maintain alone.

## Decision

- **Backend: Python with FastAPI.** Python is my main scripting language, FastAPI is
  fast to write, validates input with type hints and generates API docs automatically.
- **Database: PostgreSQL.** One database covers normal app data, time-series telemetry
  (TimescaleDB extension) and AI search (pgvector), so there is less to run and operate.
- **Frontend: React with TypeScript.** I already know React; TypeScript catches mistakes
  early. Tailwind and shadcn/ui give a clean UI without designing every component myself.
- **Platform: Docker, kind, Helm and Argo CD.** This matches how Kubernetes workloads are
  deployed in production: containers, packaged as Helm charts, delivered with GitOps.
- **Infrastructure as code: Terraform.** The most common IaC tool, and it works with
  AWS, Alibaba Cloud and local Kubernetes.
- **AI: Ollama with a small local model.** Free, private, and runs on the laptop GPU.

## Alternatives considered

- **Django instead of FastAPI:** more built in, but heavier for an API-only backend.
- **MongoDB instead of PostgreSQL:** flexible, but incidents, releases and SLOs are
  relational data, and I would lose TimescaleDB and pgvector.
- **minikube instead of kind:** both work; kind is lighter and starts multi-node
  clusters quickly inside Docker.
- **A hosted AI API:** better models, but costs money and sends data outside.

## Consequences

- Everything runs offline and for free.
- Several moving parts (database, cluster, monitoring) compete for 16 GB RAM, so I will
  not run everything at the same time.
- The AI features must keep working when the model is off, because a small local model
  can be slow or wrong.