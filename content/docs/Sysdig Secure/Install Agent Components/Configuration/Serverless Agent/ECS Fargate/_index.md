---
linkTitle: "ECS Fargate"
title: "Configure ECS Fargate Serverless Agent"
weight: "11"
no_list: true
aliases:
  - /en/config-serverless-agent-ecs-fargate
  - /en/docs/installation/configuration/ecs-fargate-serverless-agent/
description: After deploying the Sysdig serverless agent for ECS Fargate, you can configure the starting policy, enable http proxy, fetch secrets, upload a custom CA certificate, and manage logs.
---

- [Configure Priority Modes](/en/configure-priority-modes)

  You can use environment variables to customize the workload priority modes for Serverless Agents on ECS Fargate.

- [Enable HTTP Proxy for Serverless Agents](/en/enable-ecs-http-proxy)

  Configure both the orchestrator and the Workload Agent to connect to an HTTP proxy.

- [Fetching Secrets from Secrets Manager](/en/ecs-fetching-secrets)

  You can retrieve the Sysdig Access Key and the HTTP Proxy password from the AWS Secrets Manager when deploying the Orchestrator Agent as of [serverless agent version 4.0.0](/en/docs/release-notes/serverless-agent-release-notes/#400-february-10-2023).

- [Manage Serverless Agent Logs](/en/ecs-manage-logs)

  Even if the Workload Agent runs in the same container as the workload it instruments, their log streams are handled separately.

- [Uploading Custom CA Certificates](/en/ecs-uploading-custom-cert)

  As of [Serverless Agent v4.0.0](/en/docs/release-notes/serverless-agent-release-notes/2023-archive/#400-february-10-2023), the Orchestrator Agent supports the uploading of CA certificates.
