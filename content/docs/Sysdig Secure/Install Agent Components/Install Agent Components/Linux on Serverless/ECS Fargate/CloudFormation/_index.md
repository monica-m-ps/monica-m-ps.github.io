---
linkTitle: "CloudFormation"
title: "CloudFormation"
weight: "1"
no_list: true
aliases:
  - /en/install-secure-serverless-ecs-fargate-cft
  - /en/install-ecs-serverless-cft
description: "Deploy the Orchestrator Agent and Workload Agent components on ECS Fargate using a CloudFormation template stack in the AWS console. You can run the template instrumentation service either in the cloud or locally and integrate it into your CI/CD pipeline."
---

Sysdig provides two YAML templates for the Serverless Agent 4.0 and onwards:

- [orchestrator-agent.yaml](https://download.sysdig.com/dependencies/serverless/fargate/orchestrator-agent.yaml): Deploys the stack running the Orchestrator Agent
- [instrumentation.yaml](https://download.sysdig.com/dependencies/serverless/fargate/instrumentation.yaml): Deploys the instrumentation service that instruments your workload template.

The instrumentation service leverages [serverless-patcher](/en/install-ecs-serverless-patcher), a Sysdig containerized tool that automates the instrumentation CloudFormation templates.

Run the template instrumentation service either in the cloud or locally and integrate it into your CI/CD pipeline.

## Prerequisites

Review the [Installation Requirements](/en/install-requirements-secure) before getting started.

### Deploy the Orchestrator Agent {#orchestrator}

1. Log in to the AWS Console, select CloudFormation, and create a stack with new resources.

2. Upload the `orchestrator-agent.yaml` as the template file.

3. Provide the parameters highlighted in the figure, such as the `VPC` where your service is running.

4. Complete the stack creation and wait for the deployment to complete.

    ![](/image/serverless-orchestrator-agent-stack-parameters.png)

1. When complete, note the **orchestrator host** and **port** as output.

### Deploy the Instrumentation Stack

1. Push [the latest image](https://quay.io/repository/sysdig/serverless-patcher?tab=tags&tag=latest) of `serverless-patcher` to a private ECR repository within the same region where the deployment will take place.

2. Log in to the AWS Console, select CloudFormation, and create a stack with new resources.

3. Specify the `instrumentation.yaml` as the Template source.

   - The name of the Macro must be unique in your account.
   - The *serverless-patcher* image must be hosted in an ECR private repo within the same region in which the deployment takes place.

4. Provide the stack details to deploy the Instrumentation Service:

   - **Instrumentation Priority**: Supported options are **availability** (by default) or **security**.
     - In security mode, the Workload Agent aims to process every `syscall` event and will prevent the workloads from running unsecured. Consequently, secured applications will not execute until the Workload Agent receives the runtime policies.
     - The availability mode instead facilitates resource sharing between the Workload Agent and the workload containers, enabling the Workload Agent to detect when resource pressure exceeds configurable limits and pause event processing to reduce pressure.
   - **Sidecar Essential Flag**: If marked as essential, ECS will stop all containers in the task if the sidecar exits. The sidecar container is marked as essential in security mode by default to prevent secured workloads from running unsecured.
   - **Sidecar CPU Units and Memory Limit/Reservation**: The settings can be adjusted to allocate dedicated CPU units and memory resources/limits to the sidecar container.  

6. Complete the stack creation and wait for the deployment to complete.

    ![](/image/serverless-instrumentation-stack-parameters.png)

7. When complete, note the **Transformation Macro string** as an output.

   The **Outputs** tab provides the transformation instruction as given below.

   ![](/image/serverless-instrumentation-stack-output.png)

### Instrument the Workload Stack

The instrumentation process includes inspecting the images of the containers in the TaskDefinition to patch the entrypoint and command of them. If your workload containers are using images from private registries, you must explicitly provide the original entrypoint and command for each container.

1. Copy and paste the Transformation Macro string from the Sysdig Instrumentation stack output to the root level of your workload stack template.
2. Deploy your instrumented workload template. The Sysdig instrumentation service will go over the workload template to instrument it.

## Upgrade the Agents

### Orchestrator Agent

The Orchestrator Agent version can be upgraded by applying a ChangeSet to the Orchestrator CFN stack.

1. Navigate to the Orchestrator Stack.
2. Choose **Update** and **Use current template**.
3. Edit **Sysdig Orchestrator Agent Image** to choose the new image tag, if the stack was originally deployed with a specific tag.

   If the `latest` tag was used, this field can be left unchanged.
5. Redeploy the stack.

After the deployment is completed, confirm the version by navigating to the `SysdigAgentCluster` ECS cluster and reviewing the Task's Image URI.

### Workload Agent

The Workload Agent version can be upgraded by updating the Instrumentation Stack with the latest image of `serverless-patcher` specified as **Sysdig Serverless Patcher Image** and Workload Agent specified as **Sysdig Workload Agent Image**. These versions do not need to match but it is recommended to use consistent versions.

Once the Instrumentation Stack has been updated, the Workload Stack should be deleted and recreated.

## Next Steps

After the deployment is completed, security-related events will be visible in the [Sysdig Secure Events](/en/events-feed) feed.

Optionally, you can perform [advanced Configuration steps](/en/config-serverless-agent-ecs-fargate).
