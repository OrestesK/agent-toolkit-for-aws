---
name: aws-containers
description: Use for deploying, operating, or troubleshooting containerized AWS workloads with ECS, Fargate, ECR, or ECS Express Mode
version: 1
allowed-tools: [Read]
---
# AWS Containers

Use this skill as the entry point for Amazon ECS, AWS Fargate, Amazon ECR, ECS Express Mode, and existing App Runner workloads. Read only the reference that matches the current task before giving detailed guidance or commands.

## Choose the service

| Need | Default |
|---|---|
| Simple new HTTP app or API with minimal infrastructure | ECS Express Mode |
| Production web service, worker, batch job, or scheduled task | ECS on Fargate |
| GPU, instance storage, or more than 16 vCPU | ECS on EC2 |
| Container image storage | ECR |
| Kubernetes | Do not use this skill; use an EKS/Kubernetes workflow |

Do not recommend EKS unless the user explicitly needs Kubernetes. App Runner is sunset for new customers; use its reference only for existing services or migrations.

## Workflow

1. Identify whether the task concerns image storage, task definitions, service deployment, scaling, logging, debugging, or migration.
2. Inspect the existing task definition, service, cluster, repository, networking, and deployment configuration before proposing a change.
3. Read the matching reference below. Do not load unrelated references.
4. Explain the intended effect and verify prerequisites before commands.
5. Obtain explicit approval before any AWS mutation. Prefer a plan/validate/execute sequence because ECS has no general `--dry-run` mode.
6. Verify service events, task state, target health, logs, and deployment status after an approved change.

## Core invariants

- Fargate task definitions require a valid CPU/memory combination, `awsvpc` networking, and platform `LATEST` or `1.4.0`
- Keep the execution role separate from the task role: the ECS agent uses the execution role; application code and ECS Exec use the task role
- Secrets are resolved when a task starts; rotate them with a new deployment rather than expecting hot reload
- For ALB services, configure health-check grace time, a deployment circuit breaker with rollback, and an appropriate deregistration delay
- Private-subnet tasks need NAT or the required VPC endpoints; ECR image pulls also require the S3 gateway endpoint
- Confirm before `--force-new-deployment`, deleting a service, or deregistering task definitions

## Reference routing

- [task-definition-authoring.md](references/task-definition-authoring.md) — CPU/memory, networking, roles, secrets, volumes, dependencies, and platform versions
- [fargate-service-deployment.md](references/fargate-service-deployment.md) — Fargate services, ALB health checks, deployment safety, and private networking
- [ecr-repository-management.md](references/ecr-repository-management.md) — lifecycle policies, scanning, cross-account pulls, and image-pull failures
- [ecs-exec-debugging.md](references/ecs-exec-debugging.md) — ECS Exec setup, permissions, logging, and `TargetNotConnectedException`
- [service-scaling-and-updates.md](references/service-scaling-and-updates.md) — scaling, rolling or blue/green deployments, circuit breakers, and Service Connect
- [ecs-logging-and-firelens.md](references/ecs-logging-and-firelens.md) — `awslogs`, blocking versus non-blocking delivery, FireLens, and multiline logs
- [ecs-troubleshooting-guide.md](references/ecs-troubleshooting-guide.md) — placement, OOM, health checks, stopped tasks, image pulls, and networking failures
- [ecs-infrastructure-patterns.md](references/ecs-infrastructure-patterns.md) — CDK and CloudFormation examples
- [fargate-spot.md](references/fargate-spot.md) — Spot capacity strategies and interruption handling
- [app-runner-guide.md](references/app-runner-guide.md) — existing App Runner services and migration to ECS Express Mode

## Troubleshooting order

For an unhealthy or stuck service, inspect in this order:

1. ECS service events
2. stopped-task reason and container exit code
3. target-group health and application listen address
4. CloudWatch logs and log-driver behavior
5. task/execution roles, subnet routes, security groups, and VPC endpoints

Use the troubleshooting reference for the concrete failure instead of guessing from the symptom alone.

## Security

Use IAM roles rather than embedded credentials, Secrets Manager or SSM Parameter Store for secrets, least-privilege policies, ECR scanning, private networking where appropriate, and CloudTrail for API auditing. Do not log secrets or sensitive payloads. Treat internet-facing ALBs as a separate TLS/WAF boundary.
