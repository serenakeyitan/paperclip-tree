---
title: "AWS ECS Fargate Deployment"
owners: [bingran-you, cryppadotta, serenakeyitan, neerazz]
soft_links: [infrastructure/deployment/NODE.md, infrastructure/deployment/bind-presets/NODE.md]
---

Runbook and reference assets for deploying Paperclip to AWS using ECS Fargate (compute), RDS Postgres 17 (database), and EFS (persistent storage), fronted by an ALB with HTTPS.

## Key Decisions

### Single-Task ECS Service
The runbook deploys a single-task ECS Fargate service (2048 CPU / 4096 memory) behind an ALB. This matches Paperclip's current single-instance assumption — heartbeat scheduling and SQLite-style local state in `PAPERCLIP_HOME` are not designed for horizontal scaling, so a single task with EFS-backed persistence is the supported topology.

### Secrets via AWS Secrets Manager
`DATABASE_URL`, `BETTER_AUTH_SECRET`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, and `GITHUB_TOKEN` are injected through the ECS task definition's `secrets` block (Secrets Manager ARNs), never set in `.env.aws` or the task's `environment` block. The companion `docker/.env.aws.example` documents this boundary explicitly.

### Deployment Mode
The reference task definition runs `PAPERCLIP_DEPLOYMENT_MODE=authenticated` with `PAPERCLIP_DEPLOYMENT_EXPOSURE=public`, matches the public-cloud recommendation in the deployment overview, and enables `PAPERCLIP_MIGRATION_AUTO_APPLY=true` plus `HEARTBEAT_SCHEDULER_ENABLED=true` so the task self-migrates on startup and runs scheduled heartbeats.

## Assets

- `docker/.env.aws.example` — environment template for AWS deployments (non-secret config only).
- `docker/ecs-task-definition.json` — reference ECS task definition with placeholders for account ID, region, and Secrets Manager ARNs.
- `docs/deploy/aws-ecs.md` — full step-by-step runbook (ECR, RDS, EFS, ALB, ACM, ECS service).
