---
name: signing-in-to-aws
description: Use when AWS CLI or SDK credentials are missing, expired, or need configuration through `aws login`
---
# Sign In — Get CLI/SDK Credentials

Use `aws login` for local CLI or SDK credentials when no existing organizational SSO workflow applies. Run authentication commands only in the user's local shell, never through MCP or an API tool.

## Core flow

1. Run `aws --version`. `aws login` requires AWS CLI 2.32.0 or later. If it is missing or older, explain the requirement and ask before installing or updating it.
2. Run `aws sts get-caller-identity` for the intended profile.
   - If it succeeds, show the account and ARN, then ask whether to keep that identity or switch profiles
   - If credentials are missing or expired, recommend `aws login` immediately
3. Before invoking `aws login` or `aws login --profile <name>`, state the exact command and ask for confirmation. Do not tell the user to run it themselves.
4. After login, rerun `aws sts get-caller-identity` with the same profile. For a named profile, remind the user to pass `--profile` or set `AWS_PROFILE`.

`aws login` creates short-term credentials that rotate automatically and remain valid for up to 12 hours.

## Existing SSO and other boundaries

- If the user already uses IAM Identity Center, preserve that workflow and use `aws sso login --profile <name>` instead of redirecting them to `aws login`
- Do not use this workflow for CI/CD credentials; use IAM roles or OIDC federation
- `aws login` is unavailable in GovCloud and China partitions
- Offer `aws configure` only when the user declines `aws login`, requests an alternative, or uses a partition where `aws login` is unavailable

Read [references/troubleshooting.md](references/troubleshooting.md) only for installation, browser, permission, SSO, partition, or fallback problems.
