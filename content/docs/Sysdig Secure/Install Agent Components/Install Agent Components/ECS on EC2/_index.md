---
linkTitle: "ECS on EC2"
title: "ECS on EC2"
weight: "7"
no_list: true
aliases:
  - /en/install/ecs-ec2-secure/
  - /en/docs/installation/sysdig-agent/agent-installation/agent-install-ecs/
description: Amazon Elastic Container Service (ECS) is a managed container orchestration service that simplifies the deployment, management, and scaling of containerized applications. This section explains how to install the Sysdig agent container on each host within your ECS cluster. After installation, the agent will automatically begin runtime threat detection across all hosts, services, and tasks. 
---

{{% callout type="note" %}}

These instructions are valid only for ECS clusters using Elastic Compute Cloud (EC2) instances. For information on ECS Fargate clusters, see [AWS Fargate Serverless Agents](/en/install-ecs-fargate-secure/).

{{% /callout %}}

## Prerequisites

- Review
   - [Installation Requirements](/en/install-reqs-agent-secure/)
   - [Understand Agent Drivers](/en/understand-agent-drivers)
- Collect the following:
   - [Sysdig access key](/en/docs/administration/administration-settings/agent-installation-overview-and-key/)
   - [Collector address](/en/docs/administration/saas-regions-and-ip-ranges/)
- Ensure you have an EC2 cluster set up in AWS.

### Overview

To install the Sysdig agent on ECS, follow these steps:

1. [Create an ECS task definition](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html) for the Sysdig agent.
2. Register the task definition in your AWS account.
3. Create a service with the previous task definition to run the Sysdig agent on each of the nodes of your ECS cluster.

## Deployment

1. Create an ECS task definition.

   Use the values from the prerequisites to customize the JSON snippet below and save it as a file named `sysdig-agent-ecs.json`.

   Note that `memory` and `cpu` have both been set to 1024; depending on the size of your cluster you might want to tune those values. 

   To use the kernel module driver:
   ```json
     {
       "family": "sysdig-agent-ecs",
       "containerDefinitions": [
         {
           "name": "sysdig-agent",
           "image": "quay.io/sysdig/agent-slim",
           "cpu": 1024,
           "memory": 1024,
           "privileged": true,
           "environment": [
             {
               "name": "ACCESS_KEY",
               "value": "$ACCESS_KEY"
             },
             {
               "name": "COLLECTOR",
               "value": "$COLLECTOR"
             },
             {
               "name": "TAGS",
               "value": "$TAG1,TAG2"
             }
           ],
           "mountPoints": [
             {
               "readOnly": true,
               "containerPath": "/host/boot",
               "sourceVolume": "boot"
             },
             {
               "containerPath": "/host/dev",
               "sourceVolume": "dev"
             },
             {
               "readOnly": true,
               "containerPath": "/host/lib/modules",
               "sourceVolume": "modules"
             },
             {
               "readOnly": true,
               "containerPath": "/host/proc",
               "sourceVolume": "proc"
             },
             {
               "containerPath": "/host/var/run/docker.sock",
               "sourceVolume": "sock"
             },
             {
               "readOnly": true,
               "containerPath": "/host/usr",
               "sourceVolume": "usr"
             }
           ],
           "dependsOn": [
             {
               "containerName": "sysdig-agent-kmodule",
               "condition": "SUCCESS"
             }
           ]
         },
         {
           "name": "sysdig-agent-kmodule",
           "image": "quay.io/sysdig/agent-kmodule",
           "memory": 512,
           "privileged": true,
           "essential": false,
           "mountPoints": [
             {
               "readOnly": true,
               "containerPath": "/host/boot",
               "sourceVolume": "boot"
             },
             {
               "containerPath": "/host/dev",
               "sourceVolume": "dev"
             },
             {
               "readOnly": true,
               "containerPath": "/host/lib/modules",
               "sourceVolume": "modules"
             },
             {
               "readOnly": true,
               "containerPath": "/host/proc",
               "sourceVolume": "proc"
             },
             {
               "containerPath": "/host/var/run/docker.sock",
               "sourceVolume": "sock"
             },
             {
               "readOnly": true,
               "containerPath": "/host/usr",
               "sourceVolume": "usr"
             }
           ]
         }
       ],
       "pidMode": "host",
       "networkMode": "host",
       "volumes": [
         {
           "name": "sock",
           "host": {
             "sourcePath": "/var/run/docker.sock"
           }
         },
         {
           "name": "dev",
           "host": {
             "sourcePath": "/dev/"
           }
         },
         {
           "name": "proc",
           "host": {
             "sourcePath": "/proc/"
           }
         },
         {
           "name": "boot",
           "host": {
             "sourcePath": "/boot/"
           }
         },
         {
           "name": "modules",
           "host": {
             "sourcePath": "/lib/modules/"
           }
         },
         {
           "name": "usr",
           "host": {
             "sourcePath": "/usr/"
           }
         }
       ],
       "requiresCompatibilities": [
         "EC2"
       ]
     }
   ```
   
   To use the eBPF driver:
   ```json
     {
       "family": "sysdig-agent-ecs",
       "containerDefinitions": [
         {
           "name": "sysdig-agent",
           "image": "quay.io/sysdig/agent-slim",
           "cpu": 1024,
           "memory": 1024,
           "privileged": true,
           "environment": [
             {
               "name": "ACCESS_KEY",
               "value": "$ACCESS_KEY"
             },
             {
               "name": "COLLECTOR",
               "value": "$COLLECTOR"
             },
             {
               "name": "TAGS",
               "value": "$TAG1,TAG2"
             },
             {
               "name": "SYSDIG_BPF_PROBE",
               "value": ""
             }
           ],
           "mountPoints": [
             {
               "readOnly": true,
               "containerPath": "/host/boot",
               "sourceVolume": "boot"
             },
             {
               "containerPath": "/host/dev",
               "sourceVolume": "dev"
             },
             {
               "readOnly": true,
               "containerPath": "/host/lib/modules",
               "sourceVolume": "modules"
             },
             {
               "readOnly": true,
               "containerPath": "/host/proc",
               "sourceVolume": "proc"
             },
             {
               "containerPath": "/host/var/run/docker.sock",
               "sourceVolume": "sock"
             },
             {
               "readOnly": true,
               "containerPath": "/host/usr",
               "sourceVolume": "usr"
             },
             {
               "readOnly": true,
               "containerPath": "/sys/kernel/debug",
               "sourceVolume": "debug"
             }
           ],
           "dependsOn": [
             {
               "containerName": "sysdig-agent-kmodule",
               "condition": "SUCCESS"
             }
           ]
         },
         {
           "name": "sysdig-agent-kmodule",
           "image": "quay.io/sysdig/agent-kmodule",
           "memory": 512,
           "privileged": true,
           "environment": [
              {
               "name": "SYSDIG_BPF_PROBE",
               "value": ""
             }
           ],
           "essential": false,
           "mountPoints": [
             {
               "readOnly": true,
               "containerPath": "/host/boot",
               "sourceVolume": "boot"
             },
             {
               "containerPath": "/host/dev",
               "sourceVolume": "dev"
             },
             {
               "readOnly": true,
               "containerPath": "/host/lib/modules",
               "sourceVolume": "modules"
             },
             {
               "readOnly": true,
               "containerPath": "/host/proc",
               "sourceVolume": "proc"
             },
             {
               "containerPath": "/host/var/run/docker.sock",
               "sourceVolume": "sock"
             },
             {
               "readOnly": true,
               "containerPath": "/host/usr",
               "sourceVolume": "usr"
             }
           ]
         }
       ],
       "pidMode": "host",
       "networkMode": "host",
       "volumes": [
         {
           "name": "sock",
           "host": {
             "sourcePath": "/var/run/docker.sock"
           }
         },
         {
           "name": "dev",
           "host": {
             "sourcePath": "/dev/"
           }
         },
         {
           "name": "proc",
           "host": {
             "sourcePath": "/proc/"
           }
         },
         {
           "name": "boot",
           "host": {
             "sourcePath": "/boot/"
           }
         },
         {
           "name": "modules",
           "host": {
             "sourcePath": "/lib/modules/"
           }
         },
         {
           "name": "usr",
           "host": {
             "sourcePath": "/usr/"
           }
         },
         {
           "name": "debug",
           "host": {
             "sourcePath": "/sys/kernel/debug"
           }
         }
       ],
       "requiresCompatibilities": [
         "EC2"
       ]
     }
   ```

   To use the Universal eBPF driver:
   ```json
     {
       "family": "sysdig-agent-ecs",
       "containerDefinitions": [
         {
           "name": "sysdig-agent",
           "image": "quay.io/sysdig/agent-slim",
           "cpu": 1024,
           "memory": 1024,
           "privileged": true,
           "environment": [
             {
               "name": "ACCESS_KEY",
               "value": "$ACCESS_KEY"
             },
             {
               "name": "COLLECTOR",
               "value": "$COLLECTOR"
             },
             {
               "name": "TAGS",
               "value": "$TAG1,TAG2"
             },
             {
               "name": "SYSDIG_AGENT_DRIVER",
               "value": "universal_ebpf"
             }
           ],
           "mountPoints": [
             {
               "containerPath": "/host/dev",
               "sourceVolume": "dev"
             },
             {
               "readOnly": true,
               "containerPath": "/host/proc",
               "sourceVolume": "proc"
             },
             {
               "containerPath": "/host/var/run/docker.sock",
               "sourceVolume": "sock"
             },
             {
               "readOnly": true,
               "containerPath": "/sys/kernel/debug",
               "sourceVolume": "debug"
             }
           ]
         }
       ],
       "pidMode": "host",
       "networkMode": "host",
       "volumes": [
         {
           "name": "sock",
           "host": {
             "sourcePath": "/var/run/docker.sock"
           }
         },
         {
           "name": "dev",
           "host": {
             "sourcePath": "/dev/"
           }
         },
         {
           "name": "proc",
           "host": {
             "sourcePath": "/proc/"
           }
         },
         {
           "name": "debug",
           "host": {
             "sourcePath": "/sys/kernel/debug"
           }
         }
       ],
       "requiresCompatibilities": [
         "EC2"
       ]
     }
   ```

2. Register a task definition. 

   Once your task definition is ready, ensure that you register it in your AWS account:

   ```yaml
   aws ecs register-task-definition \
     --cli-input-json file://sysdig-agent-ecs.json
   ```

3. Run the agent as an ECS service. 

   Using the ECS task definition you have created, create a service in the cluster where you want Sysdig monitor and perform threat detection. 

   ```yaml
   aws ecs create-service \
       --cluster $CLUSTER_NAME \
       --service-name sysdig-agent-svc \
       --launch-type EC2 \
       --task-definition sysdig-agent-ecs \
       --scheduling-strategy DAEMON
   ```

   With the agent installed, Sysdig will begin auto-discovering your containers and other resources of your ECS environment.
