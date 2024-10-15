---
linkTitle: "Workload Agent in an Existing Image"
title: "Embed Workload Agent in an Existing Image"
weight: "4"
no_list: true
aliases:
  - /en/install-secure-serverless-embedded
description: You can include the Sysdig Workload Agent in a container image at build time, instead of relying on manual or automatic task instrumentation. To do this, update your Dockerfile to copy the required files and specify the orchestrator that you want to use with environment variables.
---

## Prerequisites

Ensure that the Serverless Orchestrator Agent is installed. Use one of the following:

- Deploy Serverless Orchestrator on ECS Fargate using [Cloudformation](/en/install-secure-serverless-ecs-fargate-cft#orchestrator)
- Deploy Serverless Orchestrator on ECS Fargate using [Terraform](/en/install-secure-serverless-ecs-fargate-tf#orchestrator)

## Deploy the Workload Agent

You can modify an existing `Dockerfile`  to include the Workload Agent.

Consider the following example image:

   ```
   FROM falcosecurity/event-generator:latest
   ENTRYPOINT ["/bin/event-generator"]
   CMD ["run", "syscall", "--all", "--loop"]
   ```

1. Modify the image as follows:

   ```yaml
   ARG SYSDIG_AGENT_VERSION=latest
   FROM quay.io/sysdig/workload-agent:${SYSDIG_AGENT_VERSION} AS workload-agent
   
   FROM falcosecurity/event-generator:latest
   
   COPY --from=workload-agent /opt/draios /opt/draios
   ENV SYSDIG_ORCHESTRATOR=orchestrator.elb.us-east-1.amazonaws.com \
       SYSDIG_ORCHESTRATOR_PORT=6667
   ENTRYPOINT ["/opt/draios/bin/instrument"]
   CMD ["/bin/event-generator", "run", "syscall", "--all", "--loop"]
   ```

2. Update your Dockerfile to copy the Sysdig Workload Agent files into your container image.

   - `COPY`: Use the `COPY` command to copy the `/opt/draios` directory  from the Sysdig Workload Agent image into your container image. 
   - `ARG`:  Specifies the version of the Sysdig Workload Agent to use, which defaults to the latest version if not specified. 
   - `FROM` : Pulls the Sysdig Workload Agent image.

3. Modify the `ENTRYPOINT` of your image to be `/opt/draios/bin/instrument` and prepend the original entrypoint to the `CMD`.

4. Specify the Sysdig orchestrator you want to use by setting the `SYSDIG_ORCHESTRATOR` and `SYSDIG_ORCHESTRATOR_PORT` environment variables in your Dockerfile.

5. Replace `orchestrator.elb.us-east-1.amazonaws.com` and `6667` with the appropriate values for your Sysdig orchestrator.

6. Build and push the instrumented container image to your container registry. 

   Ensure that the architecture of the image matches the CPU architecture of your Serverless Runtime Platform. Note that the Serverless Agent currently only supports the `x86_64` architecture.

## Next Steps

- After the deployment completes, security-related events will be visible in the [Sysdig Secure Events](/en/events-feed) feed.
- Optionally, you can perform [advanced configuration steps](/en/config-serverless-agent-ecs-fargate).
