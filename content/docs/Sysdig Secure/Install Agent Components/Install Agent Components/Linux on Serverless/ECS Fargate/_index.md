---
linkTitle: "ECS on Fargate"
title: "ECS on Fargate"
weight: "1"
no_list: true
aliases:
  - /en/install-ecs-fargate-secure
  - /en/docs/installation/sysdig-secure/install-agent-components/ecs-on-fargate/serverless-agents/
  - /en/install-ecs-serverless-manual/
  - /en/install-ecs-serverless-secure
  - /en/docs/installation/sysdig-secure/install-agent-components/ecs-on-fargate/
  - /en/docs/sysdig-secure/install-agent-components/install-agent-components/ecs-on-fargate/serverless-agents/
description: The Sysdig Secure Elastic Container Service (ECS) Fargate serverless agent provides runtime detection and policy enforcement for serverless workloads on AWS Fargate on ECS. It uses Falco to ensure the security and compliance of the workloads. The agent has two components, the Orchestrator Agent and the Workload Agent, with their own installation requirements.
---

**The Sysdig Serverless Orchestrator Agent** is installed on each virtual private cloud (VPC) and collects data from the Workload Agent(s). It sends the collected data to the Sysdig backend and syncs the Falco runtime policies and rules from the backend to the Workload Agent(s).

**The Sysdig serverless Workload Agent** is installed in each task and requires network access to communicate with the Orchestrator Agent. It monitors the serverless workload and enforces the Falco policies and rules to detect and prevent security threats and compliance violations.

![](/image/serverless_architecture.png)

By default, the Workload Agent prioritizes availability over security to ensure the workload can start even if policies are not in place. However, users can change this setting by configuring their workload starting policy to prioritize security over availability.

The Sysdig Secure ECS Fargate agent allows users to ensure the security and compliance of their serverless workloads on AWS Fargate on ECS, reducing the risk of security incidents and compliance violations.

## Prerequisites

Before starting the installation, ensure that you have the following:

#### On AWS

- A custom Terraform/CloudFormation template containing the Fargate task definitions that you want to instrument through the Sysdig Serverless Agent
- Two VPC subnets in different availability zones that can connect with the internet via a NAT gateway or an internet gateway

#### On Sysdig

- Sysdig Secure up and running

- The endpoint of the [Sysdig Collector for your region](/en/docs/administration/saas-regions-and-ip-ranges/)

- From the Sysdig Secure UI, retrieve the Access Key to install the agent and push the data to the Sysdig platform

## Limitations

#### CPU Architecture Support

Serverless Agent supports only the `amd64` architecture. `aarch64` is currently unsupported.

#### Pulling Workload Images from Public vs Private registries

Sysdig instruments a target workload by patching its TaskDefinition to run the original workload below Sysdig instrumentation. To patch the TaskDefinition, the Sysdig instrumentation service pulls and analyzes the workload image to get the original entrypoint and the command, along with other information.

- If your workload image is hosted in a private registry: you must explicitly define the entrypoint and the command in the ContainerDefinition in the template. Otherwise, the Sysdig instrumentation might not be able to collect such information and the instrumentation might fail.
- If your workload image is hosted in a public registry: no additional operations are required.

### Compatibility Matrix
|              | Orchestrator 5.0.0 | Orchestrator 4.3 | Orchestrator 4.2 | Orchestrator 4.1 |
| -------------| -------------------| ---------------- | ---------------- | ---------------- |
| Workload 5.0 |           &#x2705; |         &#x2705; |         &#x2705; |         &#x2705; |
| Workload 4.3 |           &#x2705; |         &#x2705; |         &#x2705; |         &#x2705; |
| Workload 4.2 |           &#x274c; |         &#x2705; |         &#x2705; |         &#x2705; |
| Workload 4.1 |           &#x274c; |         &#x2705; |         &#x2705; |         &#x2705; |


## Next Steps

You can deploy the Sysdig ECS Fargate Serverless Agent in different ways, depending on your preferences and requirements. There are two deployment options, automated or manual, and two automated methods - Terraform or CloudFormation.

- **Automatic deployment via Terraform or CloudFormation (Recommended):**
  You can automate the deployment of the Sysdig serverless agent using Terraform or CloudFormation templates. This method simplifies the deployment process, ensures consistency across deployments, and simplifies ongoing maintenance.

  [Install on ECS Fargate using Terraform](/en/install-secure-serverless-ecs-fargate-tf)

  [Install on ECS Fargate using CloudFormation](/en/install-secure-serverless-ecs-fargate-cft)

- **Manual deployment:**
  You can manually deploy the Sysdig serverless agent by following the installation instructions provided by Sysdig. This method requires more effort and is prone to human error, but provides more flexibility and control over the deployment process.

  [Install on ECS Fargate manually](/en/install-secure-serverless-ecs-fargate-manual)

With the recommended deployment method, you can quickly and easily instrument your Fargate workloads with the Sysdig serverless agent, ensuring runtime detection and policy enforcement for improved security and compliance.
