---
linkTitle: "Event Forwarding"
title: "Event Forwarding"
weight: "10"
aliases:
  - /en/event-forwarding.html
  - /en/event-forwarding/
description: "Sysdig Secure can send security data to third-party platforms and logging tools such as Splunk, Qradar, and Elastic. Use Event Forwarding integrations to view security events and correlate Sysdig findings with the tool you are already using for analysis."
---
Sysdig supports both **standard event forwarding** and **agent local forwarding** options.

## Supported Data Types

Standard event forwarding and agent local forwarding support different data types.

| Supported Data Types                           | Standard | Agent Local | Notes                                                        |
| ---------------------------------------------- | -------- | ----------- | ------------------------------------------------------------ |
| Runtime Policy events                          | ✅       | ✅          |                                                              |
| Activity Audit                                 | ✅       | ✅          |                                                              |
| Sysdig Platform Audit                          | ✅       |             |                                                              |
| Monitor events                                 | ✅       |             | If Sysdig Monitor is installed                               |
| Older legacy policy events format (Deprecated) | ✅       |             |                                                              |
| Legacy Compliance v1 (Deprecated)              | ✅       |             | Forwarding options are called "Secure events compliance" and "Benchmark events" |
| Legacy Vulnerability Scanner v1 (Deprecated)   | ✅       |             | The forwarding option is called "Host Scanning"              |

## Standard Event Forwarding

Each subpage in this section describes how to use the Sysdig UI to configure forwarding events to designated third-party systems, including open-ended integrations using Webhook or Syslog. These integrations pass the data through the Sysdig backend and forward to external systems using applicable APIs.

{{% callout type="note" %}}

You must be logged in to Sysdig Secure as an Administrator to access the event forwarding options.

{{% /callout %}}

### Add Standard Integrations

1. Log in to Sysdig Secure as **Admin** and go to **Settings** > **Event Forwarding**.

   ![](/image/efo.png)

2. Click **+Add Integration** and choose a listed integration, or Syslog or Webhook, and complete the relevant integration fields in UI.

### Delete Standard Integrations

To delete an existing integration:

1. Log in to Sysdig Secure as Admin and go to **Profile > Settings > Event Forwarding**.

2. Click the **More Options** (three dots) icon.

3. Click **Delete Integration**.

4. Click  **Yes, delete** to confirm.

## Agent Local Forwarding

With **agent v.12.18.0+**, Sysdig supports the possibility of forwarding data directly from the Sysdig agent, avoiding the Sysdig backend before landing on the target platform.

**Benefits** of the agent local option:

- Avoid sending data out of your own environment.
- Avoid exposing a locally hosted SIEM to the internet.

**Key differences** from the standard event forwarding option:

- The local forwarder does not support X.509 authentication.
- Events are not persisted, and therefore are lost if not forwarded during an agent restart.
- Some labels might not be available as they can be populated in a skipped post-processing phase.
- Description and agentId fields are not available.
- Requires manual configuration of agent config files rather than UI entry fields.

### Configure Agent Local Forwarding

#### Enable the Forwarder

Edit the agent `values.yaml` (Helm) or `dragent.yaml` (non-Helm) to contain the settings to enable the forwarder and to define what data to send to it:

For `values.yaml` (Helm)

```
localForwarder:
  enabled: true
  transmitMessageTypes:
   - POLICY_EVENTS
   - SECURE_AUDIT
```

For `dragent.yaml` (non-Helm)

```
local_forwarder:
  enabled: true
  transmit_message_types:
   - POLICY_EVENTS
   - SECURE_AUDIT
```

`Message_types` can be either or both options.

#### Supported Types

Channels are not available on every type:

| **Type**  | **Runtime policy events** | **Activity Audit** |
| --------- | ------------------------- | ------------------ |
| CHRONICLE | ✅                        | ❌                 |
| ELASTIC   | ✅                        | ✅                 |
| KAFKA     | ✅                        | ✅                 |
| MCM       | ✅                        | ❌                 |
| PUBSUB    | ✅                        | ✅                 |
| QRADAR    | ✅                        | ✅                 |
| SCC       | ✅                        | ❌                 |
| SENTINEL  | ✅                        | ✅                 |
| SPLUNK    | ✅                        | ✅                 |
| SQS       | ✅                        | ✅                 |
| SYSLOG    | ✅                        | ✅                 |
| WEBHOOK   | ✅                        | ✅                 |

#### Configure the Target Parameters

Add the configuration details for a selected integration.

- **Helm:** If you are using Helm, add to your `values.yaml` file under the `Integrations` config parameter.
- **Non-Helm:** If you are not using Helm, then add the configuration details in another file located in the same directory as the  `dragent.yaml`:  `local_forwarder_config.yaml`.

The integration entries for each type follow this sample format:

```
integrations:
- type: SPLUNK
  channels:
  - SECURE_EVENTS_POLICIES
  - ACTIVITY_AUDIT
  configuration:
    Index: indexname
    ServiceToken: ***
    ServiceURL: "https://yoursplunkurl.com"
```

Check the **Agent Local Forwarding** section on each subpage for the details of that type. For example, see  [Splunk](/en/forward-splunk).

## Reference: JSON Formats Used per Data Source

Informational; in most cases, there is no need to change the default format.

### Policy Event Payload

#### Policy Event Severity

The severity field in the payload is an integer. The following table shows different values event severities can have.

| Event Severity     | JSON severity value |
| ------------------ | ------------------- |
| High               | 0, 1, 2, 3          |
| Medium             | 4, 5                |
| Low                | 6                   |
| Info               | 7                   |

There are now two formats supported. See also the [Release Note of December 11, 2020](/en/docs/release-notes/saas-sysdig-secure-release-notes/2020-archive/#december-11-2020). The Legacy format has been deprecated as of [Jan 18, 2023](/en/docs/release-notes/saas-sysdig-secure-release-notes/#january-18-2024) and its removal will occur in accordance with a separate announcement.

{{% callout type="note" %}}

To learn about Sysdig Monitor event severity levels, see [Severity and Status](/en/docs/sysdig-monitor/events/severity-and-status/).

{{% /callout %}}

#### New Runtime Policy Events Payload

```yaml
{
    "id": "164ace360cc3cfbc26ec22d61b439500",
    "type": "policy",
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "originator": "policy",
    "category": "runtime",
    "source": "syscall",
    "rawEventOriginator": "linuxAgent",
    "rawEventCategory": "runtime",
    "sourceDetails": {
      "sourceType": "workload",
      "sourceSubType": "host"
    },
    "engine": "falco",
    "name": "Notable Filesystem Changes",
    "description": "Identified notable filesystem activity that might change sensitive/important files. This differs from Suspicious Filesystem Changes in that it looks more broadly at filesystem activity, and might have more false positives as a result.",
    "severity": 0,
    "agentId": 13530,
    "containerId": "",
    "machineId": "08:00:27:54:f3:9d",
    "actions": [
        {
          "type": "POLICY_ACTION_CAPTURE",
          "successful": true,
          "token": "abffffdd-fba8-42c7-b922-85364b00eeeb",
          "afterEventNs": 5000000000,
          "beforeEventNs": 5000000000
        }
    ],
    "content": {
        "policyId": 544,
        "baselineId": "",
        "ruleName": "Write below etc",
        "ruleType": "RULE_TYPE_FALCO",
        "ruleTags": [
            "NIST_800-190",
            "NIST_800-53",
            "ISO",
            "NIST_800-53_CA-9",
            "NIST_800-53_SC-4",
            "NIST",
            "ISO_27001",
            "MITRE_T1552_unsecured_credentials",
            "MITRE_T1552.001_credentials_in_files"
        ],
        "output": "File below /etc opened for writing (user=root command=touch /etc/ard parent=bash pcmdline=bash file=/etc/ard program=touch gparent=su ggparent=sudo gggparent=bash container_id=host image=<NA>)",
        "fields": {
            "container.id": "host",
            "container.image.repository": "<NA>",
            "falco.rule": "Write below etc",
            "fd.directory": "/etc/pam.d",
            "fd.name": "/etc/ard",
            "group.gid": "8589935592",
            "group.name": "sysdig",
            "proc.aname[2]": "su",
            "proc.aname[3]": "sudo",
            "proc.aname[4]": "bash",
            "proc.cmdline": "touch /etc/ard",
            "proc.name": "touch",
            "proc.pcmdline": "bash",
            "proc.pname": "bash",
            "user.name": "root"
        },
        "falsePositive": false,
        "matchedOnDefault": false,
        "policyVersion": 2,
        "policyOrigin": "Sysdig"
    },
    "labels": {
        "host.hostName": "ardbox",
        "process.name": "touch /etc/ard"
    }
}
```

#### (Deprecated) Legacy Secure Policy Event Payload

```yaml
{
    "id": "164ace360cc3cfbc26ec22d61b439500",
    "containerId": "",
    "name": "Notable Filesystem Changes",
    "description": "Identified notable filesystem activity that might change sensitive/important files. This differs from Suspicious Filesystem Changes in that it looks more broadly at filesystem activity, and might have more false positives as a result.",
    "severity": 0,
    "policyId": 544,
    "actionResults": [
        {
            "type": "POLICY_ACTION_CAPTURE",
            "successful": true,
            "token": "15c6b9cc-59f9-4573-82bb-a1dbab2c4737",
            "beforeEventNs": 5000000000,
            "afterEventNs": 5000000000
        }
    ],
    "output": "File below /etc opened for writing (user=root command=touch /etc/ard parent=bash pcmdline=bash file=/etc/ard program=touch gparent=su ggparent=sudo gggparent=bash container_id=host image=<NA>)",
    "ruleType": "RULE_TYPE_FALCO",
    "matchedOnDefault": false,
    "fields": [
        {
            "key": "container.image.repository",
            "value": "<NA>"
        },
        {
            "key": "proc.aname[3]",
            "value": "sudo"
        },
        {
            "key": "proc.aname[4]",
            "value": "bash"
        },
        {
            "key": "proc.cmdline",
            "value": "touch /etc/ard"
        },
        {
            "key": "proc.pname",
            "value": "bash"
        },
        {
            "key": "falco.rule",
            "value": "Write below etc"
        },
        {
            "key": "proc.name",
            "value": "touch"
        },
        {
            "key": "fd.name",
            "value": "/etc/ard"
        },
        {
            "key": "proc.aname[2]",
            "value": "su"
        },
        {
            "key": "proc.pcmdline",
            "value": "bash"
        },
        {
            "key": "container.id",
            "value": "host"
        },
        {
            "key": "user.name",
            "value": "root"
        }
    ],
    "eventLabels": [
        {
            "key": "container.image.repo",
            "value": "alpine"
        },
        {
            "key": "container.image.tag",
            "value": "latest"
        },
        {
            "key": "container.name",
            "value": "large-label-container-7"
        },
        {
            "key": "host.hostName",
            "value": "ardbox"
        },
        {
            "key": "process.name",
            "value": "touch /etc/ard"
        }
    ],
    "falsePositive": false,
    "baselineId": "",
    "policyVersion": 2,
    "origin": "Sysdig",
    "timestamp": 1606322948648718,
    "timestampNs": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "hostMac": "08:00:27:54:f3:9d",
    "isAggregated": false
}
```

### Activity Audit Forwarding Payloads

Each of the activity audit types has its own JSON format.

#### Command (cmd) Payload

```yaml
{
    "id": "164806c17885b5615ba513135ea13d79",
    "agentId": 32212,
    "cmdline": "calico-node -felix-ready -bird-ready",
    "comm": "calico-node",
    "pcomm": "apt-get",
    "containerId": "a407fb17332b",
    "count": 1,
    "customerId": 1,
    "cwd": "/",
    "hostname": "qa-k8smetrics",
    "loginShellDistance": 0,
    "loginShellId": 0,
    "pid": 29278,
    "ppid": 29275,
    "procExepath": "/usr/bin/calico-node",
    "rxTimestamp": 1606322949537513500,
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "tty": 34816,
    "type": "command",
    "uid": 0,
    "username": "root",
    "userLoginUid": 4294967295,
    "userLoginName": "<NA>",
    "labels": {
        "aws.accountId": "059797578166",
        "aws.instanceId": "i-053b1f0509fdbc15a",
        "aws.region": "us-east-1",
        "container.image.digest": "sha256:26c68657ccce2cb0a31b330cb0be2b5e108d467f641c62e13ab40cbec258c68d",
        "container.image.id": "d2e4e1f51132",
        "container.label.io.kubernetes.pod.namespace": "default",
        "container.name": "bash",
        "host.hostName": "ip-172-20-46-221",
        "host.mac": "12:9f:a1:c9:76:87",
        "kubernetes.node.name": "ip-172-20-46-221.ec2.internal",
        "kubernetes.pod.name": "bash"
    }
}
```

#### Network (net) Payload

```yaml
{
    "id": "164806f43b4d7e8c6708f40cdbb47838",
    "agentId": 32212,
    "clientIpv4": 2886795285,
    "clientIpv4Dot": "172.17.0.21",
    "clientPort": 60720,
    "comm": "kubectl",
    "containerId": "da3abd373c7a",
    "customerId": 1,
    "direction": "out",
    "dnsDomains": [
      "api.openai.com"
    ],
    "errorCode": 0,
    "hostname": "qa-k8smetrics",
    "l4protocol": 6,
    "pid": 2452,
    "processName": "kubectl",
    "rxTimestamp": 0,
    "serverIpv4": 174063617,
    "serverIpv4Dot": "10.96.0.1",
    "serverPort": 443,
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "type": "connection"
    "tty": 34816,
    "labels": {
        "aws.accountId": "059797578166",
        "aws.instanceId": "i-053b1f0509fdbc15a",
        "aws.region": "us-east-1",
        "container.image.digest": "sha256:26c68657ccce2cb0a31b330cb0be2b5e108d467f641c62e13ab40cbec258c68d",
        "container.image.id": "d2e4e1f51132",
        "host.hostName": "ip-172-20-46-221",
        "host.mac": "12:9f:a1:c9:76:87",
        "kubernetes.cluster.name": "k8s-onprem",
        "kubernetes.namespace.name": "default",
        "kubernetes.node.name": "ip-172-20-46-221.ec2.internal",
        "kubernetes.pod.name": "bash"
    }
}
```

#### File (file) Payload

```yaml
{
    "id": "164806c161a5dd221c4ee79d6b5dd1ce",
    "agentId": 32212,
    "containerId": "a407fb17332b",
    "directory": "/var/lib/dpkg/updates/",
    "filename": "tmp.i",
    "hostname": "qa-k8smetrics",
    "permissions": "w",
    "pid": 414661,
    "comm": "dpkg",
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "type": "fileaccess",
    "labels": {
        "aws.accountId": "059797578166",
        "aws.instanceId": "i-053b1f0509fdbc15a",
        "aws.region": "us-east-1",
        "container.image.digest": "sha256:26c68657ccce2cb0a31b330cb0be2b5e108d467f641c62e13ab40cbec258c68d",
        "container.image.id": "d2e4e1f51132",
        "container.image.repo": "docker.io/library/ubuntu",
        "container.name": "bash",
        "host.hostName": "ip-172-20-46-221",
        "host.mac": "12:9f:a1:c9:76:87",
        "kubernetes.cluster.name": "k8s-onprem",
        "kubernetes.namespace.name": "default",
        "kubernetes.node.name": "ip-172-20-46-221.ec2.internal",
        "kubernetes.pod.name": "bash"
    }
}
```

#### Kubernetes (kube exec) Payload

```yaml
{
    "id": "164806f4c47ad9101117d87f8b574ecf",
    "agentId": 32212,
    "args": {
        "command": "bash",
        "container": "nginx"
    },
    "auditId": "c474d1de-c764-445a-8142-a0142505868e",
    "containerId": "397be1762fba",
    "hostname": "qa-k8smetrics",
    "name": "nginx-76f9cf7469-k5kf7",
    "namespace": "nginx",
    "resource": "pods",
    "sourceAddresses": [
        "172.17.0.21"
    ],
    "stages": {
        "started": 1605540915526159000,
        "completed": 1605540915660084000
    },
    "subResource": "exec",
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "type": "kubernetes",
    "user": {
        "username": "system:serviceaccount:default:default-kubectl-trigger",
        "groups": [
            "system:serviceaccounts",
            "system:serviceaccounts:default",
            "system:authenticated"
        ]
    },
    "userAgent": "kubectl/v1.16.2 (linux/amd64) kubernetes/c97fe50",
    "labels": {
        "agent.tag.cluster": "k8s-onprem",
        "agent.tag.sysdig_secure.enabled": "true",
        "container.image.repo": "docker.io/library/nginx",
        "container.image.tag": "1.21.6",
        "container.label.io.kubernetes.container.name": "nginx",
        "container.label.io.kubernetes.pod.name": "nginx-76f9cf7469-k5kf7",
        "container.label.io.kubernetes.pod.namespace": "nginx",
        "container.name": "nginx",
        "host.hostName": "qa-k8smetrics",
        "host.mac": "12:09:c7:7d:8b:25",
        "kubernetes.cluster.name": "demo-env-prom",
        "kubernetes.deployment.name": "nginx-deployment",
        "kubernetes.namespace.name": "nginx",
        "kubernetes.pod.name": "nginx-76f9cf7469-k5kf7",
        "kubernetes.replicaSet.name": "nginx-deployment-5677bff5b7"
    }
}
```

### Sysdig Platform Audit Payload

```yaml
{
    "id": "16f43920a0d70f005f136173fcec3375",
    "type": "audittrail",
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "originator": "ingestion",
    "category": "",
    "source": "auditTrail",
    "name": "",
    "description": "",
    "severity": 0,
    "agentId": 0,
    "containerId": "",
    "machineId": "",
    "content": {
        "timestampNs": 1654009775452000000,
        "customerId": 1,
        "userId": 454926,
        "teamId": 46902,
        "requestMethod": "GET",
        "requestUri": "/api/integrations/discovery/",
        "userOriginIP": "187.188.243.122",
        "queryString": "cluster=demo-env-prom&namespace=sysdig-agent",
        "responseStatusCode": 200,
        "entityType": "integration",
        "entityPayload": ""
    },
    "labels": {
        "entityType": "integration"
    }
}
```

### (Deprecated) Benchmark Result Payloads

To forward benchmark events, you must have **Benchmarks v2**
[installed](/en/install-reqs-host-scan) and configured,
using the Node Analyzer.

A Benchmark Control payload is emitted for each control on each host on
every Benchmark Run. A Benchmark Run payload containing a summary of the
results is emitted for each host on every Benchmark Run.

#### Benchmark Control Payload

```yaml
{
    "id": "16ee684c65c356616381cbcbfed06eb6",
    "type": "benchmark",
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "originator": "benchmarks",
    "category": "runtime",
    "source": "host",
    "name": "Kubernetes Benchmark Control Reported",
    "description": "Kubernetes benchmark kube_bench_cis-1.6.0 control 4.1.8 completed.",
    "severity": 7,
    "agentId": 0,
    "containerId": "",
    "machineId": "0a:e2:ce:65:f5:b7",
    "content": {
        "taskId": "9",
        "runId": "535de4fb-3fac-4716-b5c6-9c906226ed01",
        "source": "host",
        "schema": "kube_bench_cis-1.6.0",
        "subType": "control",
        "control": {
            "id": "4.1.8",
            "title": "Ensure that the client certificate authorities file ownership is set to root:root (Manual)",
            "description": "The certificate authorities file controls the authorities used to validate API requests. You should set its file ownership to maintain the integrity of the file. The file should be owned by `root:root`.",
            "rationale": "The certificate authorities file controls the authorities used to validate API requests. You should set its file ownership to maintain the integrity of the file. The file should be owned by `root:root`.",
            "remediation": "Run the following command to modify the ownership of the --client-ca-file.\nchown root:root <filename>\n",
            "auditCommand": "CAFILE=$(ps -ef | grep kubelet | grep -v apiserver | grep -- --client-ca-file= | awk -F '--client-ca-file=' '{print $2}' | awk '{print $1}')\nif test -z $CAFILE; then CAFILE=/etc/kubernetes/pki/ca.crt; fi\nif test -e $CAFILE; then stat -c %U:%G $CAFILE; fi\n",
            "auditOutput": "root:root",
            "expectedOutput": "'root:root' is equal to 'root:root'",
            "familyName": "Worker Node Configuration Files",
            "level": "Level 1",
            "type": "manual",
            "result": "Pass",
            "resourceType": "Hosts",
            "resourceCount": 0
        }
    },
    "labels": {
        "aws.accountId": "845151661675",
        "aws.instanceId": "i-0cafe61565a04c866",
        "aws.region": "eu-west-1",
        "host.hostName": "ip-172-20-57-8",
        "host.mac": "0a:e2:ce:65:f5:b7",
        "kubernetes.cluster.name": "demo-env-prom",
        "kubernetes.node.name": "ip-172-20-57-8.eu-west-1.compute.internal"
    }
}
```

#### Benchmark Run Payload

```yaml
{
    "id": "16ee684c65c356617457f59f07b11210",
    "type": "benchmark",
    "timestamp": 1606322948648718268,
    "timestampRFC3339Nano": "2020-11-25T16:49:08.648718268Z",
    "originator": "benchmarks",
    "category": "runtime",
    "source": "host",
    "name": "Kubernetes Benchmark Run Passed (with warnings)",
    "description": "Kubernetes benchmark kube_bench_cis-1.6.0 completed.",
    "severity": 4,
    "agentId": 0,
    "containerId": "",
    "machineId": "0a:28:16:38:93:39",
    "content": {
        "taskId": "9",
        "runId": "535de4fb-3fac-4716-b5c6-9c906226ed01",
        "source": "host",
        "schema": "kube_bench_cis-1.6.0",
        "subType": "run",
        "run": {
            "passCount": 20,
            "failCount": 0,
            "warnCount": 27
        }
    },
    "labels": {
        "aws.accountId": "845151661675",
        "aws.instanceId": "i-00280f61718cc25ba",
        "aws.region": "eu-west-1",
        "host.hostName": "ip-172-20-40-177",
        "host.mac": "0a:28:16:38:93:39",
        "kubernetes.cluster.name": "demo-env-prom",
        "kubernetes.node.name": "ip-172-20-40-177.eu-west-1.compute.internal"
    }
}
```

### (Deprecated) Host Scanning Payload

#### Incremental Report

This is the "vuln diff" report; it contains the list of added, removed,
or updated vulnerabilities that the host presents compared to the
previous scan.

```yaml
[
  {
    "id": "167fddc1197bcc776d72f0f299e83530",
    "type": "hostscanning",
    "timestamp": 1621258212302,
    "originator": "hostscanning",
    "category": "hostscanning_incremental_report",
    "source": "hostscanning",
    "name": "Vulnerability updates - Host dev-vm",
    "description": "",
    "severity": 4,
    "agentId": 0,
    "containerId": "",
    "machineId": "00:0c:29:e5:9e:51",
    "content": {
      "hostname": "dev-vm",
      "mac": "00:0c:29:e5:9e:51",
      "reportType": "incremental",
      "added": [
        {
          "cve": "CVE-2020-27170",
          "fixAvailable": "5.4.0-70.78",
          "packageName": "linux-headers-5.4.0-67",
          "packageType": "dpkg",
          "packageVersion": "5.4.0-67.75",
          "severity": "High",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2020-27170",
          "vulnerablePackage": "linux-headers-5.4.0-67:5.4.0-67.75"
        },
        {
          "cve": "CVE-2019-9515",
          "fixAvailable": "None",
          "packageName": "libgrpc6",
          "packageType": "dpkg",
          "packageVersion": "1.16.1-1ubuntu5",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2019-9515",
          "vulnerablePackage": "libgrpc6:1.16.1-1ubuntu5"
        }
      ],
      "updated": [
        {
          "cve": "CVE-2018-17977",
          "fixAvailable": "None",
          "packageName": "linux-modules-5.4.0-72-generic",
          "packageType": "dpkg",
          "packageVersion": "5.4.0-72.80",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2018-17977",
          "vulnerablePackage": "linux-modules-5.4.0-72-generic:5.4.0-72.80"
        },
        {
          "cve": "CVE-2021-3348",
          "fixAvailable": "5.4.0-71.79",
          "packageName": "linux-modules-extra-5.4.0-67-generic",
          "packageType": "dpkg",
          "packageVersion": "5.4.0-67.75",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2021-3348",
          "vulnerablePackage": "linux-modules-extra-5.4.0-67-generic:5.4.0-67.75"
        },
        {
          "cve": "CVE-2021-29265",
          "fixAvailable": "5.4.0-73.82",
          "packageName": "linux-headers-5.4.0-67-generic",
          "packageType": "dpkg",
          "packageVersion": "5.4.0-67.75",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2021-29265",
          "vulnerablePackage": "linux-headers-5.4.0-67-generic:5.4.0-67.75"
        },
        {
          "cve": "CVE-2021-29921",
          "fixAvailable": "None",
          "packageName": "python3.8-dev",
          "packageType": "dpkg",
          "packageVersion": "3.8.5-1~20.04.2",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2021-29921",
          "vulnerablePackage": "python3.8-dev:3.8.5-1~20.04.2"
        }
      ],
      "removed": [
        {
          "cve": "CVE-2021-26932",
          "fixAvailable": "None",
          "packageName": "linux-modules-5.4.0-67-generic",
          "packageType": "dpkg",
          "packageVersion": "5.4.0-67.75",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2021-26932",
          "vulnerablePackage": "linux-modules-5.4.0-67-generic:5.4.0-67.75"
        },
        {
          "cve": "CVE-2020-26541",
          "fixAvailable": "None",
          "packageName": "linux-modules-extra-5.4.0-67-generic",
          "packageType": "dpkg",
          "packageVersion": "5.4.0-67.75",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2020-26541",
          "vulnerablePackage": "linux-modules-extra-5.4.0-67-generic:5.4.0-67.75"
        },
        {
          "cve": "CVE-2014-4607",
          "fixAvailable": "2.04-1ubuntu26.8",
          "packageName": "grub-pc",
          "packageType": "dpkg",
          "packageVersion": "2.04-1ubuntu26.7",
          "severity": "Medium",
          "url": "http://people.ubuntu.com/~ubuntu-security/cve/CVE-2014-4607",
          "vulnerablePackage": "grub-pc:2.04-1ubuntu26.7"
        }
      ]
    },
    "labels": {
      "host.hostName": "dev-vm",
      "cloudProvider.account.id": "",
      "cloudProvider.host.name": "",
      "cloudProvider.region": "",
      "host.hostName": "ip-172-20-40-177",
      "host.id": "d82e5bde1d992bedd10a640bdb2f052493ff4b3e03f5e96d1077bf208f32ea96",
      "host.mac": "00:0c:29:e5:9e:51",
      "host.os.name": "ubuntu",
      "host.os.version": "20.04"
      "kubernetes.cluster.name": "",
      "kubernetes.node.name": ""
    }
  }
]
```

#### Full Report

The full report contains all the vulnerabilities found during the first
host scan.

```yaml
[
  {
    "id": "1680c8462f368eaf38d2f269d9de1637",
    "type": "hostscanning",
    "timestamp": 1621516069618,
    "originator": "hostscanning",
    "category": "hostscanning_full_report",
    "source": "hostscanning",
    "name": "Host ip-172-31-94-81 scanned",
    "description": "",
    "severity": 4,
    "agentId": 0,
    "containerId": "",
    "machineId": "16:1f:b4:f5:02:03",
    "content": {
      "hostname": "ip-172-31-94-81",
      "mac": "16:1f:b4:f5:02:03",
      "reportType": "full",
      "added": [
        {
          "cve": "CVE-2015-0207",
          "fixAvailable": "None",
          "packageName": "libssl1.1",
          "packageType": "dpkg",
          "packageVersion": "1.1.0l-1~deb9u3",
          "severity": "Negligible",
          "url": "https://security-tracker.debian.org/tracker/CVE-2015-0207",
          "vulnerablePackage": "libssl1.1:1.1.0l-1~deb9u3"
        },
        {
          "cve": "CVE-2016-2088",
          "fixAvailable": "None",
          "packageName": "libdns162",
          "packageType": "dpkg",
          "packageVersion": "1:9.10.3.dfsg.P4-12.3+deb9u8",
          "severity": "Negligible",
          "url": "https://security-tracker.debian.org/tracker/CVE-2016-2088",
          "vulnerablePackage": "libdns162:1:9.10.3.dfsg.P4-12.3+deb9u8"
        },
        {
          "cve": "CVE-2017-5123",
          "fixAvailable": "None",
          "packageName": "linux-headers-4.9.0-15-amd64",
          "packageType": "dpkg",
          "packageVersion": "4.9.258-1",
          "severity": "Negligible",
          "url": "https://security-tracker.debian.org/tracker/CVE-2017-5123",
          "vulnerablePackage": "linux-headers-4.9.0-15-amd64:4.9.258-1"
        },
        {
          "cve": "CVE-2014-2739",
          "fixAvailable": "None",
          "packageName": "linux-headers-4.9.0-15-common",
          "packageType": "dpkg",
          "packageVersion": "4.9.258-1",
          "severity": "Negligible",
          "url": "https://security-tracker.debian.org/tracker/CVE-2014-2739",
          "vulnerablePackage": "linux-headers-4.9.0-15-common:4.9.258-1"
        },
        {
          "cve": "CVE-2014-9781",
          "fixAvailable": "None",
          "packageName": "linux-kbuild-4.9",
          "packageType": "dpkg",
          "packageVersion": "4.9.258-1",
          "severity": "Negligible",
          "url": "https://security-tracker.debian.org/tracker/CVE-2014-9781",
          "vulnerablePackage": "linux-kbuild-4.9:4.9.258-1"
        },
        {
          "cve": "CVE-2015-8705",
          "fixAvailable": "None",
          "packageName": "libisc-export160",
          "packageType": "dpkg",
          "packageVersion": "1:9.10.3.dfsg.P4-12.3+deb9u8",
          "severity": "Negligible",
          "url": "https://security-tracker.debian.org/tracker/CVE-2015-8705",
          "vulnerablePackage": "libisc-export160:1:9.10.3.dfsg.P4-12.3+deb9u8"
        }
      ]
    },
    "labels": {
      "agent.tag.distribution": "Debian",
      "agent.tag.fqdn": "ec2-3-231-219-145.compute-1.amazonaws.com",
      "agent.tag.test-type": "qa-hs",
      "agent.tag.version": "9.13",
      "host.hostName": "ip-172-31-94-81",
      "host.id": "cbd8fc14e9116a33770453e0755cbd1e72e4790e16876327607c50ce9de25a4b",
      "host.mac": "16:1f:b4:f5:02:03",
      "host.os.name": "debian",
      "host.os.version": "9.13"
      "kubernetes.cluster.name": "",
      "kubernetes.node.name": ""
    }
  }
]
```
