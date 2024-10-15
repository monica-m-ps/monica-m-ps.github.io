---
linkTitle: "Agent Configuration for Secure"
title: "Agent Configuration for Secure"
weight: "11"
aliases:
  - /en/configuration-library-secure
  - /en/docs/installation/configuration/sysdig-agent/configuration-library/secure/
Description: "The Sysdig configuration library lists all the major configurations required to enable Sysdig Secure features."
---

## Configure Malware Control

{{% callout type="note" %}}

This feature is available in Technical Preview status.

{{% /callout %}}

Malware Control, which is comprised of Malware Detection and Prevention, is enabled by default for containers. For hosts, Malware Detection is enabled by default, but Malware Prevention is disabled by default.

You can configure the agent to:

* Disable Malware Control globally.
* Enabled Malware Prevention for hosts.

To configure the agent, see [Understand the Agent Configuration](/en/understand-the-agent-configuration).

### Prerequisites

- Sysdig Agent v13.0.1 and above
- Linux kernel v5.0 and above

### Enable or Disable Malware Control

To enable Malware Control globally, use this configuration:

```
malware_control:
  enabled: true
```

Alternatively, use the following Helm command:

```
--set sysdig.settings.malware_control.enabled=true
```

To disable Malware Control globally, use this configuration:

```
malware_control:
  enabled: false
```

### Enable or Disable Malware Prevention for Hosts


Malware Prevention, by default, is disabled for Hosts. To enable it, use the following configuration:

```yaml
protections:
  malware_control:
    enable_prevention_on_host: true
```

To disable Malware Prevention on hosts, use the following configuration:

```yaml
protections:
  malware_control:
    enable_prevention_on_host: false
```

### Configure Container Limits

- For kernel versions below v5.13, Malware can monitor up to 128 containers per node.

- For kernel versions v5.13 or above, modify the container limit using one of the following methods:

  - Open the `sysctl -n fs.fanotify.max_user_groups` file and set the new value using `sysctl -w fs.fanotify.max_user_groups=<new_limit>`.

  - Check the current limit using `cat /proc/sys/fs/fanotify/max_user_groups` file and use `echo <new_limit> > /proc/sys/fs/fanotify/max_user_groups` to set the new limit.


## Configure Drift Control

#### Sysdig Agent v12.15.0+

Drift is enabled by default on agent versions v12.15.0 and later.

##### Optional Values

Enable detections from [mounted/persistent volumes](/en/docs/sysdig-secure/policies/threat-detect-policies/manage-policies/container-drift/#persistent-volumes).

###### Configuration

```
drift_deny_execution_from_volumes: true
```

###### Helm Command

```
--set agent.sysdig.settings.drift_deny_execution_from_volumes=true
```

#### Sysdig Agent v12.14.0

This configuration is deprecated for newer agent versions.

##### Configuration

###### Enable Drift

```yaml
drift_killer:
  enabled: true
```
###### Disable Drift

```yaml
drift_killer:
  enabled: false
```
Here is an example configuration:

```yaml
agent:
  ebpf:
    enabled: "true"
    kind: "universal_ebpf"
  sysdig:
    settings:
      enrich_with_process_lineage: "true"
      drift_control:
        enabled: false 

```

##### Helm Command

```
 --set agent.sysdig.settings.drift_killer.enabled=true
```

### Configure Container Limits 

- For kernel versions below v5.13, Drift Control can monitor up to 128 containers per node.

- For kernel versions v5.13 or above, modify the container limit using one of the following methods:

  - Open the `sysctl -n fs.fanotify.max_user_groups` file and set the new value using `sysctl -w fs.fanotify.max_user_groups=<new_limit>`.

  - Open the `cat /proc/sys/fs/fanotify/max_user_groups` file and run `echo <new_limit> > /proc/sys/fs/fanotify/max_user_groups`.

    Replace `<new_limit>` with your choice of container limit.

## Configure Falco Rule Matching Strategy

#### Sysdig agent v.12.18+

From Sysdig agent v12.18.0+, the agent evaluates an event against all the rules, potentially triggering multiple alerts. In previous versions, the agent stopped evaluating rules after the first match.

To control this behavior, a new option has been added to `dragent.yaml`: `security.falco_match_strategy`

```yaml
security:
  falco_match_strategy: all
```

To evaluate all rules for every event; set it to `all`. This is the default option.

To stop evaluation after the first match; set it to `first`.

## Report Actions in Kubernetes Events

#### Sysdig agent v.12.18+

For a full description of the feature, see [Threat Detection Policies](/en/docs/sysdig-secure/policies/threat-detect-policies/#report-policy-actions-in-kubernetes-events).

**Permissions**

* **Helm:** If you deploy the agent using Helm, the permissions to enable `create` and `patch` actions for events on all APIs are automatically granted.

* **Manual:**  If you deploy manually, you must set up a Kubernetes cluster role with those permissions enabled.
  Example without cluster role binding:

  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    name: sysdig-agent
  rules:
  - apiGroups:
    - ""
    resources:
    - events
    verbs:
    - create
    - patch
  ```

  Example with cluster role binding:

  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    name: sysdig-agent
  rules:
  - apiGroups:
    - ""
    resources:
    - events
    verbs:
    - create
    - patch
  ---
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
    name: sysdig-agent
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: ClusterRole
    name: sysdig-agent
  subjects:
  - kind: ServiceAccount
    name: sysdig-agent
    namespace: sysdig-agent
  ---
  ```

## Ignore Container Actions at the Agent Level 

#### Sysdig agent v12.10+

For Threat Detection policies, you can use the agent-level configuration, `ignore_container_action` to prevent the Sysdig agent from taking potentially disruptive container operations, such as `kill`, `pause`, `stop`, regardless of the runtime threat detection policy.

This configuration is disabled by default. To enable it, add the following to the `dragent.yaml` file:

```
security:  ignore_container_action: true 
```

When the configuration is enabled, and a policy instructs the agent to perform a container operation, the agent ignores the policy and creates an `Info` log message explaining the agent did not perform the action because of the configuration.

See also [Workload Policy](/en/workload-policy) for an example of the container action at the policy level. 

## Learn More

  - [Agent Configuration for Monitor](/en/configuration-library-monitor)
  - [Configuration Library](/en/configuration-library)
