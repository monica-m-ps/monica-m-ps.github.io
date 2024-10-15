---
linkTitle: "Terraform"
title: "Terraform"
weight: "2"
no_list: true
aliases:  
  - /en/install-secure-serverless-ecs-fargate-tf
  - /en/install-ecs-serverless-tf
  - /en/docs/sysdig-secure/install-agent-components/install-agent-components/ecs-on-fargate/serverless-agents/terraform/
description: "Deploy the Orchestrator Agent and Workload Agent components on ECS Fargate using Terraform."
---

## Prerequisites

- Review the [**Installation Requirements**](/en/install-requirements-secure) before getting started.

- For details about the Resources, Inputs, and Outputs, see the following Sysdig Terraform registries:
  - [Orchestrator Agent](https://registry.terraform.io/modules/sysdiglabs/fargate-orchestrator-agent/aws/latest)
  - [Workload Agent](https://registry.terraform.io/providers/sysdiglabs/sysdig/latest/docs/data-sources/fargate_workload_agent)

- For the images pulled from private registries, explicitly provide the `Entrypoint` and `Command` in the related container definition, or the instrumentation will not be completed.

## Deploy the Agent Components

### Orchestrator Agent {#orchestrator}

1. Set up the AWS Terraform provider:

   ```
   provider "aws" {
     region = var.region
   }
   ```

2. Configure the Sysdig orchestrator module and deploy it for each VPC that needs instrumentation:

   ```terraform
   module "fargate-orchestrator-agent" {
     source  = "sysdiglabs/fargate-orchestrator-agent/aws"
     version = "0.4.1"
   
     vpc_id           = var.vpc_id
     subnets          = [var.subnet_a_id, var.subnet_b_id]
   
     access_key       = var.access_key
   
     collector_host   = var.collector_host
     collector_port   = var.collector_port
   
     name             = "sysdig-orchestrator"
     agent_image      = "quay.io/sysdig/orchestrator-agent:latest"
   
     # True if the VPC uses an InternetGateway, false otherwise
     assign_public_ip = true
   
     tags = {
       description    = "Sysdig Serverless Agent Orchestrator"
     }
   }
   
   ```

### Workload Agent

1. Set up the AWS Terraform provider:

   ```
   provider "aws" {
     region = var.region
   }
   ```

2. Set up the Sysdig Terraform provider:

      ```terraform
      terraform {
        required_providers {
          sysdig = {
            source = "sysdiglabs/sysdig"
            version = ">= 1.28.5"
          }
        }
      }
      ```

3. Pass the orchestrator host, port, and container definitions of your workload to the `sysdig_fargate_workload_agent` data source.

   Note: The input container definitions must be in JSON format:

   ```terrform
   data "sysdig_fargate_workload_agent" "instrumented" {
       container_definitions                = jsonencode([...])

       workload_agent_image                 = "quay.io/sysdig/workload-agent:latest"

       orchestrator_host                    = module.fargate-orchestrator-agent.orchestrator_host
       orchestrator_port                    = module.fargate-orchestrator-agent.orchestrator_port
 
       # Advanced configuration options
       priority                             = ""
       instrumentation_essential            = ""
       instrumentation_cpu                  = ""
       instrumentation_memory_limit         = ""
       instrumentation_memory_reservation   = ""
     }
   ```

   You can also specify advanced configuration options:
   - **Priority**:Supported options are **availability** (by default) or **security**.
       - In security mode the Workload Agent processes every syscall event and prevents the workloads from running unsecured. Consequently, secured applications will not execute until the Workload Agent receives the runtime policies.
       - The availability mode instead facilitates resource sharing between the Workload Agent and the workload containers, enabling the Workload Agent to detect when resource pressure exceeds configurable limits and pause event processing to reduce pressure.
   - **Instrumentation_essential**: If marked as essential, ECS will stop all containers in the task if the sidecar exits. The sidecar container is marked as essential in security mode by default to prevent secured workloads from running unsecured.
   - **Instrumentation cpu/memory configuration**: The settings can be adjusted to allocate dedicated CPU units and memory resources/limits to the sidecar container.

4. Include the instrumented JSON in your Fargate task definition and deploy your instrumented workload:

      ```terraform
      resource "aws_ecs_task_definition" "task_definition" {
        family             = "${var.prefix}-instrumented-task-definition"
        task_role_arn      = aws_iam_role.task_role.arn
        execution_role_arn = aws_iam_role.execution_role.arn
          
        cpu                      = "256"
        memory                   = "512"
        network_mode             = "awsvpc"
        requires_compatibilities = ["FARGATE"]
        pid_mode                 = "task"           
          
        container_definitions = data.sysdig_fargate_workload_agent.containers_instrumented.output_container_definitions
      }
      ```

     `pid_mode` is optional and is available only in versions 5.0.0 and above.

The Sysdig instrumentation will go over the original task definition to instrument it. The process includes replacing the original entry point and command of the containers.

## Upgrade the Agent Components

The Orchestrator and Workload agents can be upgraded individually by redeploying their respective stacks. If the stacks were deploying using the `latest` tag, redeploying the existing Terraform will reference the new versions.

### Orchestrator Agent

The Orchestrator Agent running in a stack can be updated by modifying the `agent_image` version in the module if the version was explicitly defined.

For example, to upgrade v4.2.0 to v5.0.0, you will add the following in your task definition:

```terraform
module "fargate-orchestrator-agent" {
  ...
  agent_image = "quay.io/sysdig/orchestrator-agent:5.0.0"
  ...
}
```

### Workload Agent

The Orchestrator Agent running in a stack can be updated by modifying the `agent_image` version in the module if the version was explicitly defined.

For example, to upgrade v4.2.0 to v5.0.0, you will add the following in your task definition:

```terraform
data "sysdig_fargate_workload_agent" "containers_instrumented" {
  ...
  workload_agent_image = "quay.io/sysdig/workload-agent:5.0.0"
  ...
}
```

To upgrade to v5.0.0, you also add a parameter `pid_mode` and set it to `task`. For example:

```terraform
resource "aws_ecs_task_definition" "task_definition" {
  family             = "${var.prefix}-instrumented-task-definition"
  task_role_arn      = aws_iam_role.task_role.arn
  execution_role_arn = aws_iam_role.execution_role.arn

  cpu                      = "256"
  memory                   = "512"
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  pid_mode                 = "task"    
  workload_agent_image     = "quay.io/sysdig/workload-agent:5.0.0"       

  container_definitions    = data.sysdig_fargate_workload_agent.containers_instrumented.output_container_definitions
}
```

## Next Steps

After the deployment completes, security-related events will be visible in the [Sysdig Secure Events](/en/events-feed) feed.

Optionally, you can perform [advanced Configuration steps](/en/config-serverless-agent-ecs-fargate).
