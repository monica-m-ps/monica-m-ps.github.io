---
linkTitle: "Manual Task Instrumentation"
title: "Manual Task Instrumentation"
weight: "3"
no_list: true
aliases:
  - /en/install-secure-serverless-ecs-fargate-manual
description: "If you prefer to manually instrument your task definition rather than using serverless-patcher or including the Sysdig Workload Agent in your container image, you can follow these steps."
---

## Deploy the Orchestrator Agent

Install the Sysdig Orchestrator Agent via [Terraform](/en/install-secure-serverless-ecs-fargate-tf) or [CloudFormation](/en/install-secure-serverless-ecs-fargate-cft). Take note of the `OrchestratorHost` and `OrchestratorPort` values, as you will need to pass these as environment variables to your containers.

## Secure the containers with the Workload Agent 5.0.0

1. Enable `task` pid mode at the task level:

   ```diff
   {
      "containerDefinitions": [...],
   +  "pidMode": "task"
   }
   ```

2. Add the Sysdig sidecar container to your existing `containerDefinitions`.

   This Sysdig sidecar container initiates the Workload Agent to secure the containers specified in the task.

   1. Give it a name, such as `sysdigInstrumentation`.

   2. Use the `quay.io/sysdig/workload-agent:latest` image for this container, and leave the `entrypoint` and `command` fields empty.

3. Provide the sidecar container with the following environment variables:

     - `SYSDIG_ORCHESTRATOR` and `SYSDIG_ORCHESTRATOR_PORT`: Specify the  values that you obtained while [Deploying the Orchestrator Agent](#deploy-the-orchestrator-agent).

     - `SYSDIG_PRIORITY`: Specify  either `availability` or `security`, depending on your use case.

     - `SYSDIG_SIDECAR` : Set to `auto`.

   This allows the Workload Agent to reach the Orchestrator Agent.

   ```diff
   {
     ...
     "containerDefinitions": [
       ...
   +   {
   +     "name": "sysdigInstrumentation",
   +     "image": "quay.io/sysdig/workload-agent:latest"
   +     "environment": [
   +       {
   +           "name": "SYSDIG_ORCHESTRATOR",
   +           "value": "orchestrator.example.com"
   +       },
   +       {
   +           "name": "SYSDIG_ORCHESTRATOR_PORT",
   +           "value": "6667"
   +       },
   +       {
   +           "name": "SYSDIG_PRIORITY",
   +           "value": "availability"
   +       },
   +       {
   +           "name": "SYSDIG_SIDECAR",
   +           "value": "auto"
   +       }
   +     ]
   +   }
     ]
   }
   ```

3. For each container you want to secure, add a `volume mount` from the `sysdigInstrumentation` sidecar container to your container.

   This provides the Sysdig's userspace instrumentation to the container.

   ```diff
   {
     ...
     "containerDefinitions": [
       {
          "name": "my-container-1",
          "image": "myapp:latest",
          "entrypoint": ["my", "original", "entrypoint"],
          ...
   +      "volumesFrom": [
   +        {
   +           "sourceContainer": "sysdigInstrumentation",
   +           "readOnly": true
   +        }
   +      ]
       }
     ]
   }
   ```

4. For each container you want to secure, add the `SYS_PTRACE` Linux capability to enable userspace instrumentation.

   ```diff
   {
     ...
     "containerDefinitions": [
       {
         "name": "my-container-1",
         "image": "myapp:latest",
         "entrypoint": ["my", "original", "entrypoint"],
         ...
         "volumesFrom": [
           {
             "sourceContainer": "sysdigInstrumentation",
             "readOnly": true
           }
         ],
   +     "linuxParameters": {
   +       "capabilities": {
   +         "add": ["SYS_PTRACE"]
   +       }
   +     }
       }
     ]
   }
   ```

5. For each container you want to secure, prepend `/opt/draios/bin/instrument` to the entrypoint.

   ```diff
   {
     ...
     "containerDefinitions": [
       {
         "name": "my-container-1",
         "image": "myapp:latest",
   +     "entrypoint": ["/opt/draios/bin/instrument", "my", "original", "entrypoint"],
         ...
         "volumesFrom": [
           {
             "sourceContainer": "sysdigInstrumentation",
             "readOnly": true
           }
         ],
         "linuxParameters": {
           "capabilities": {
             "add": ["SYS_PTRACE"]
           }
         }
       }
     ]
   }
   ```

   This enables the Sysdig instrumentation to run in the secured container.

6. Provide each container you want to secure with the environment variable `SYSDIG_SIDCAR = "auto"`

   ```diff
   {
     ...
     "containerDefinitions": [
       {
         "name": "my-container-1",
         "image": "myapp:latest",
         "entrypoint": ["/opt/draios/bin/instrument", "my", "original", "entrypoint"],
         ...
         "volumesFrom": [
           {
             "sourceContainer": "sysdigInstrumentation",
             "readOnly": true
           }
         ],
         "linuxParameters": {
           "capabilities": {
             "add": ["SYS_PTRACE"]
           }
         },
         "environment": [
           ...
   +       {
   +         "name": "SYSDIG_SIDECAR",
   +         "value": "auto"
   +       }
         ]
       }
     ]
   }
   ```

7. Save your updated task definition, and then deploy it to your ECS cluster.

### Example Instrumentation

The example guides you through manually instrumenting your task definition to deploy the Sysdig Workload Agent. While this method demands more manual configuration compared to using `serverless-patcher` or embedding the Sysdig Workload Agent in your container image, it offers greater control over the instrumentation process.

For the following generic task definition:

```
{
  "containerDefinitions": [
    {
      "name": "my-container-1",
      "image": "myapp:latest",
      "entrypoint": ["my", "original", "entrypoint"],
      "command": ["my", "original", "command"],
      "environment": [
        {
          "name": "my-envar",
          "value": "my-value"
        }
      ]
    }
  ]
}
```

The instrumented version is:

```diff
{
  "containerDefinitions": [
    {
      "name": "my-container-1",
      "image": "myapp:latest",
+     "entrypoint": ["/opt/draios/bin/instrument", "my", "original", "entrypoint"],
      "command": ["my", "original", "command"]
      "environment": [
        {
          "name": "my-envar",
          "value": "my-value"
        },
+       {
+         "name": "SYSDIG_SIDECAR",
+         "value": "auto",
+       }
      ],
+     "linuxParameters": {
+       "capabilities": {
+         "add": ["SYS_PTRACE"]
+       }
+     },
+     "volumesFrom": [
+       {
+         "sourceContainer": "sysdigInstrumentation",
+         "readOnly": true
+       }
+     ]
    },
+   {
+     "name": "sysdigInstrumentation",
+     "image": "quay.io/sysdig/workload-agent:latest"
+     "environment": [
+       {
+           "name": "SYSDIG_ORCHESTRATOR",
+           "value": "orchestrator.example.com"
+       },
+       {
+           "name": "SYSDIG_ORCHESTRATOR_PORT",
+           "value": "6667"
+       },
+       {
+           "name": "SYSDIG_PRIORITY",
+           "value": "availability"
+       },
+       {
+           "name": "SYSDIG_SIDECAR",
+           "value": "auto"
+       } 
+   }
+ ],
+ "pidMode": "task"
}
```

## Upgrade Workload Agent v4.x to 5.0

Given a generic task definition secured by the Workload Agent 4.x as the following:

```
{
   "containerDefinitions": [
      {
         "name": "my-container-1",
         "image": "myapp:latest",
         "entrypoint": ["/opt/draios/bin/instrument", "my", "original", "entrypoint"],
         "command": ["my", "original", "command"]
         "environment": [
            {
               "name": "my-envar",
               "value": "my-value"
            },
            {
               "name": "SYSDIG_ORCHESTRATOR",
               "value": "orchestrator-host-from-step-1",
            },
            {
               "name": "SYSDIG_ORCHESTRATOR_PORT",
               "value": "orchestrator-port-from-step-1",
            }
         ],
         "linuxParameters": {
            "capabilities": {
               "add": ["SYS_PTRACE"]
            }
         },
         "volumesFrom": [
            {
               "sourceContainer": "sysdigInstrumentation",
               "readOnly": true
            }
         ]
      },
      {
        "name": "sysdigInstrumentation",
        "image": "quay.io/sysdig/workload-agent:4.3.2" 
      }
   ]
}
```

1. Turn on the `task` pid mode at the task level:

   ```diff
   {
      "containerDefinitions": [...],
   +  "pidMode": "task"
   }
   ```

2. Update the Workload Agent image to v5.0.0:

   ```diff
   {
      "containerDefinitions": [
          ...,
          {
              "name": "sysdigInstrumentation",
   +          "image": "quay.io/sysdig/workload-agent:5.0.0" 
          }
      ],
      "pidMode": "task"
   }
   ```

3. Provide the following environment variables to the Sysdig sidecar container:
   - `SYSDIG_ORCHESTRATOR` and `SYSDIG_ORCHESTRATOR_PORT`: Specify the values that you obtained while [Deploying the Orchestrator Agent](#deploy-the-orchestrator-agent)

   - `SYSDIG_PRIORITY` : Specify either `availability` or `security`, depending on your use case.

   - `SYSDIG_SIDECAR`: Set to to `auto`.

     ```diff
     {
        "containerDefinitions": [
           ...,
           {
              "name": "sysdigInstrumentation",
              "image": "quay.io/sysdig/workload-agent:5.0.0"
     +        "environment": [
     +           {
     +              "name": "SYSDIG_ORCHESTRATOR",
     +              "value": "orchestrator-host-from-step-1",
     +           },
     +           {
     +              "name": "SYSDIG_ORCHESTRATOR_PORT",
     +              "value": "orchestrator-port-from-step-1",
     +           },
     +           {
     +              "name": "SYSDIG_PRIORITY",
     +              "value": "availability"
     +           },
     +           {
     +              "name": "SYSDIG_SIDECAR",
     +              "value": "auto"
     +           }
     +        ], 
           }
        ]
     }
     ```

4. Add the environment variable, `SYSDIG_SIDECAR="auto"`,  to the secured container:

   ```diff
   {
      "containerDefinitions": [
         {
            "name": "my-container-1",
            "image": "myapp:latest",
            "entrypoint": ["/opt/draios/bin/instrument", "my", "original", "entrypoint"],
            "command": ["my", "original", "command"]
            "environment": [
               ...,
   +           {
   +              "name": "SYSDIG_SIDECAR",
   +              "value": "auto",
   +           }
            ],
            "linuxParameters": {
               "capabilities": {
                  "add": ["SYS_PTRACE"]
               }
            },
            "volumesFrom": [
               {
                  "sourceContainer": "sysdigInstrumentation",
                  "readOnly": true
               }
            ]
         },
         {
            "name": "sysdigInstrumentation",
            "image": "quay.io/sysdig/workload-agent:5.0.0"
            "environment": [
               {
                  "name": "SYSDIG_ORCHESTRATOR",
                  "value": "orchestrator-host-from-step-1",
               },
               {
                  "name": "SYSDIG_ORCHESTRATOR_PORT",
                  "value": "orchestrator-port-from-step-1",
               },
               {
                  "name": "SYSDIG_PRIORITY",
                  "value": "availability"
               },
               {
                  "name": "SYSDIG_SIDECAR",
                  "value": "auto"
               }
            ], 
         }
      ]
   }
   ```

## Secure the Containers with Workload Agent v4.x

1. Add the Sysdig sidecar container to your existing task definition.

   This Sysdig sidecar container initiates the Workload Agent to secure the containers specified in the task.

   1. Give it a name, such as `sysdigInstrumentation`.

   2. Use the `quay.io/sysdig/workload-agent:4.3.2` image for this container, and leave the `entrypoint` and `command` fields empty.

      ```yaml
      {
        "name": "sysdigInstrumentation",
        "image": "quay.io/sysdig/workload-agent:4.3.2" 
      }
      ```

2. For each container you want to secure, add a `volume mount` from the `sysdigInstrumentation` sidecar container to your workload container.

   This provides the Workload Agent to the containers to secure.

   ```yaml
   "volumesFrom": [
     {
        "sourceContainer": "sysdigInstrumentation",
        "readOnly": true
     }
   ]
   ```

3. Update your container defenition to add the `SYS_PTRACE` capability to your workload container and enable userspace instrumentation:

   ```yaml
   "linuxParameters": {
     "capabilities": {
       "add": ["SYS_PTRACE"]
     }
   }
   ```

4. Enable the Workload Agent to run in the secured container. To do so, prepend `/opt/draios/bin/instrument` to the entrypoint of your workload container to secure it.

   For example, if your original entrypoint is `["my", "original", "entrypoint"]`, it becomes `["/opt/draios/bin/instrument", "my", "original", "entrypoint"]`.

5. Set the `SYSDIG_ORCHESTRATOR` and `SYSDIG_ORCHESTRATOR_PORT` environment variables in your workload container to the values that you obtained while [Deploying the Orchestrator Agent](#deploy-the-orchestrator-agent).

   This allows the Workload Agent to reach the Orchestrator Agent.

   For example:

   ```yaml
   "environment": [
     {"name": "SYSDIG_ORCHESTRATOR", "value": "orchestrator.example.com"},
     {"name": "SYSDIG_ORCHESTRATOR_PORT", "value": "6667"}
   ]
   ```

6. Save your updated task definition, and then deploy it to your ECS cluster.

### Example Instrumentation

The example guides you through manually instrumenting your task definition to deploy the Sysdig Workload Agent. While this method demands more manual configuration compared to using `serverless-patcher` or embedding the Sysdig Workload Agent in your container image, it offers greater control over the instrumentation process.

For the following generic task definition:

```diff
{
  "containerDefinitions": [
    {
      "name": "my-container-1",
      "image": "myapp:latest",
      "entrypoint": ["my", "original", "entrypoint"],
      "command": ["my", "original", "command"],
      "environment": [
        {
          "name": "my-envar",
          "value": "my-value"
        }
      ]
    }
  ]
}
```

The instrumented version is:

```diff
{
  "containerDefinitions": [
    {
      "name": "my-container-1",
      "image": "myapp:latest",
+     "entrypoint": ["/opt/draios/bin/instrument", "my", "original", "entrypoint"],
      "command": ["my", "original", "command"]
      "environment": [
        {
          "name": "my-envar",
          "value": "my-value"
        },
+       {
+         "name": "SYSDIG_ORCHESTRATOR",
+         "value": "orchestrator-host-from-step-1",
+       },
+       {
+         "name": "SYSDIG_ORCHESTRATOR_PORT",
+         "value": "orchestrator-port-from-step-1",
+       }
      ]
+     "linuxParameters": {
+       "capabilities": {
+         "add": ["SYS_PTRACE"]
+       }
+     },
+     "volumesFrom": [
+       {
+         "sourceContainer": "sysdigInstrumentation",
+         "readOnly": true
+       }
+     ]
    },
+   {
+     "name": "sysdigInstrumentation",
+     "image": "quay.io/sysdig/workload-agent:latest" 
+   }
  ]
}
```

## Next Steps

After the deployment is completed, security-related events will be visible in the [Sysdig Secure Events](/en/events-feed) feed.

Optionally, you can perform [advanced Configuration steps](/en/config-serverless-agent-ecs-fargate).
