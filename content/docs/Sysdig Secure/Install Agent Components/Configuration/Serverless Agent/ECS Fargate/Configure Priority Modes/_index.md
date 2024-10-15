---
title: "Configure Priority Modes"
linkTitle: "Configure Priority Modes"
weight: "10"
aliases:
  - /en/configure-priority-modes
  - /en/docs/installation/configuration/ecs-fargate-serverless-agent/configure-priority-modes/
Description: "You can use environment variables to customize the workload priority modes for Serverless Agents on ECS Fargate."
---

## Serverless Agent v5.0.0 and Above

You can decide if your Workload Agent needs runtime policies for initialization by configuring priority modes. Choose between `Security` or `Availability` modes to establish the initialization approach:

### Security

This mode allocates dedicated resources for the Workload Agent sidecar and attempts to capture every event. The `Security` mode prevents the secured applications from running in an unsecured manner. Consequently, secured applications will not execute commands until the Workload Agent receives the runtime policies.

### Availability

In this mode, the serverless agent facilitates resource sharing between the Workload Agent sidecar and the workload itself. The Workload Agent can detect when resource pressure exceeds a set limit. When this occurs, the agent will pause event generation to reduce pressure. This optimization allows tasks to be provisioned with lower resource requests, ensuring efficient operation of the Workload Agent with spare resources from the workload.

The `Availability` mode prioritizes the availability of the secured applications and allows them to start even without having the runtime policies in place.

### Configure Priority Modes

Use the following parameters to prioritize between `Security` and `Availability` modes:

- **`SysdigPriority`**: Enables automatic Terraform or CloudFormation installers.
- **`SYSDIG_PRIORITY`**: Enables manual instrumentation. Provide this environment variable to the Sysdig sidecar container to use manual instrumentation.

For more information on configuration, see the following:

- [Install Using Terraform](/en/install-secure-serverless-ecs-fargate-tf)
- [Install Using CloudFormation Template](/en/install-secure-serverless-ecs-fargate-cft)

## Serverless Agent Versions up to 4.3.2

In serverless agent versions up to 4.3.2, the instrumentation initiates the workload even without policies in place. This prevents workload starvation in cases of agent misconfiguration or network issues.

You can customize the workload starting policy by the following configurations:

### `agentino.run_without_policies`

This environment variable defines whether the Sysdig instrumentation should continue running  with no policies in place. `true` enables the workload to run unsecured. `false` disallows the workload to run unsecured so, the workload will not run at all without policies. The default is `true`.

### `agentino.delay_startup_until_policies_timeout_s`

This environment variable specifies the duration in seconds that Sysdig instrumentation should wait before initiating the workload. The time required for the Workload Agent to acquire policies varies based on factors like configuration, network latency, and load. A recommended value could be 60 seconds. The default is 0 (zero).

You can provide such configuration options to the Workload Agent via the `SYSDIG_EXTRA_CONF` environment variable. Note that `SYSDIG_EXTRA_CONF` expects either a valid YAML or JSON.

#### Example: Delay Workload Startup and Start Workload Agent with No Policies

```
SYSDIG_EXTRA_CONF='"agentino": {"delay_startup_until_policies_timeout_s": 60}'
```

This configuration does the following:

- Delays the workload startup for 60 secs to let Sysdig instrumentation acquire the policies
- Enables the workload to start with no policies in place

#### Example: Delay Workload Startup and Prevent Workload Agent from Starting without Policies

```
SYSDIG_EXTRA_CONF='{"agentino": {"run_without_policies": false, "delay_startup_until_policies_timeout_s": 60}}'
```

This configuration does the following:

- Delays the workload startup for 60 secs to let Sysdig instrumentation acquire the policies.
- prevents the workload from starting after the waiting if policies are not in place.
