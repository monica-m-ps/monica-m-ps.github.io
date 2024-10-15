---
linkTitle: "Serverless Patcher"
title: "Serverless Patcher"
weight: "10"
no_list: true
aliases:
  - /en/docs/installation/serverless-agents/aws-fargate-serverless-agents/serverless-patcher/
  - /en/install-ecs-serverless-patcher
description: The Sysdig [serverless-patcher](https://quay.io/repository/sysdig/serverless-patcher?tab=tags&tag=latest) tool lets you instrument your CloudFormation templates when installing the Sysdig Serverless Agent for ECS on Fargate. You can run serverless-patcher both in the cloud and locally, and it can be integrated with your CI/CD pipeline or automation tools to instrument templates automatically before deploying them to your staging environment.
---

## Overview

Patching a CloudFormation template means performing the following changes to its `TaskDefinition`:

 - Adding a sidecar container that exposes a volume to provide the Sysdig Workload Agent to the workload containers
 - Making each workload container mount a data volume from the sidecar container
 - Modifying the original entrypoint of each workload container to run the Sysdig instrumentation into it
 - Adding the Linux capability `SYS_PTRACE` to each workload container
 - Adding the Sysdig environment variables to each workload container

## Instrument your CloudFormation Templates

1. Download the latest version of [serverless-patcher](https://quay.io/repository/sysdig/serverless-patcher?tab=tags&tag=latest):

   ```
   docker pull quay.io/sysdig/serverless-patcher:latest
   ```

2. Instrument your workload template:

   ```
   docker run -e KILT_MODE="local" -e KILT_SRC_TEMPLATE="/path/to/src/template" -e KILT_OUT_TEMPLATE="/path/to/out/template" [OPTIONS] -v /host/path/template:/path/template quay.io/sysdig/serverless-patcher:latest
   ```

   The above command runs serverless-patcher locally and instruments your CloudFormation template.

   You need to replace:

   - `/path/to/src/template` with the path to your original CloudFormation template
   - `/path/to/out/template` with the path to the output instrumented template.
   - `/host/path/template` with the path to the folder containing the original template on your local machine.

   Additionally, you can pass the following environment variables to the command to configure serverless-patcher:

   - `SYSDIG_ORCHESTRATOR_HOST:` The Orchestrator Agent Host.
   - `SYSDIG_ORCHESTRATOR_PORT:` The Orchestrator Agent’s port. The default value is 6667.
   - `SYSDIG_WORKLOAD_AGENT_IMAGE`: The Workload Agent image to instrument workload containers. Each release defines the proper version.
   - `SYSDIG_LOGGING`: The Sysdig Instrumentation login level. The supported values are silent, fatal, critical, error, warning, info, debug, trace.

   Once the command finishes executing, you should have a new CloudFormation template with the same name as the original but with `_instrumented` appended to it. You can use this instrumented template to deploy your workload to AWS.

   You can use this to instrument your CloudFormation templates as part of your CI/CD pipeline.
