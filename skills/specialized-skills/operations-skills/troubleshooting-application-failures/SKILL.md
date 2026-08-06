---
name: troubleshooting-application-failures
description: Use for diagnosing application failures through CloudWatch log discovery and analysis
version: 1
---

# Application Failure Troubleshooting

## Overview

Domain expertise for diagnosing application failures through CloudWatch log analysis.
Discovers relevant log groups, searches for error patterns and stack traces, performs
root cause analysis, and generates prioritized remediation recommendations.

## Troubleshoot a failing application

To diagnose and resolve application failures using CloudWatch logs, follow the
procedure exactly. See [Application failure troubleshooting procedure](references/application-failure-troubleshooting.md).

## Troubleshooting

### No log groups found

Ask the user for specific log group names. Common patterns: `/aws/lambda/function-name`,
`/aws/apigateway/api-name`, or custom application log groups.

### Access denied errors

Verify AWS credentials have `logs:DescribeLogGroups`, `logs:DescribeLogStreams`,
`logs:StartQuery`, and `logs:GetQueryResults` permissions.

### Query timeouts

Reduce the time window or limit results. Large log groups may require multiple smaller queries.
