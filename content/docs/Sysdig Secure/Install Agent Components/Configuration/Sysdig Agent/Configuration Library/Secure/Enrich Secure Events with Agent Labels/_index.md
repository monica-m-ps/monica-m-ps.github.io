---
linkTitle: "Enrich Secure Events with Labels"
title: "Enrich Secure Events with Labels"
weight: "4"
aliases:
  - /en/event-enrichment-with-agent-labels.html
  - /en/event-forwarding/event-enrichment-with-agent-labels/
  - /en/event-labels
description: "Event labels are used in a variety of contexts in Sysdig Secure, such as Event Forwarding, Posture, and the Events feed. The Sysdig Agent collects a number of label fields by default. This page describes how to collect additional, custom labels, disable the collection of default labels, and disable label collection entirely."
---

## Add Custom Labels with the Tag Attribute

The agent has a set of default labels. Use tags to include additional labels and/or exclude labels from the default set.

```yaml
event_labels:
  exclude:
    - custom.label.to.exclude

event_labels:
  include:
    - custom.label.to.include
```

The following is an example of an enriched event being sent to Splunk:

```yaml
{
   agentId: 1658033
   category: runtime
   containerId: d9f5e4a9aedd
   content: {
     baselineId:
     falsePositive: false
     fields: {
       container.id: d9f5e4a9aedd
       container.image.repository: sysdiglabs/example-voting-app-voter
       container.name: k8s_voter_voter-77d98548bc-hmkpc_example-voting-app_d27f532a-41f5-49f3-a140-99afccbac5e4_63603
       evt.category: process
       falco.rule: Launch Root User Container
       fd.rip: <NA>
       fd.rport: <NA>
       proc.cmdline: container:d9f5e4a9aedd
       proc.name: container:d9f5e4a9aedd
       proc.pid: -1
       proc.pname: <NA>
       proc.ppid: -1
     }
     matchedOnDefault: false
     output: Outbound connection to IP/Port flagged by container:d9f5e4a9aedd (command=container:d9f5e4a9aedd port=<NA> ip=<NA> container=k8s_voter_voter-77d98548bc-hmkpc_example-voting-app_d27f532a-41f5-49f3-a140-99afccbac5e4_63603 (id=d9f5e4a9aedd) image=sysdiglabs/example-voting-app-voter) extra fields = (<NA> -1 process -1 container:d9f5e4a9aeddproc.aname container:d9f5e4a9aedd -1proc.apid )
     policyId: 10009837
     policyOrigin: Sysdig
     policyVersion: 37
     ruleName: Launch Root User Container
     ruleTags: [
       network
       mitre_execution
     ]
     ruleType: RULE_TYPE_FALCO
   }
   description: This Notable Events policy contains rules which may indicate undesired behavior including security threats. The rules are more generalized than Threat Detection policies and may result in more noise. Tuning will likely be required for the events generated from this policy.
   id: 1726f87daaaee3960301e17f9b06c3cf
   labels: {
     agent.tag.role: demo-kube-eks
     aws.accountId: <account-id>
     aws.instanceId: <instance-id>
     aws.region: us-east-1
     container.image.digest: <sha-256>
     container.image.id: <image-id>
     container.image.repo: sysdiglabs/example-voting-app-voter
     container.image.tag: 0.1
     container.label.io.kubernetes.container.name: voter
     container.label.io.kubernetes.pod.name: voter-77d98548bc-hmkpc
     container.label.io.kubernetes.pod.namespace: example-voting-app
     container.name: k8s_voter_voter-77d98548bc-hmkpc_example-voting-app_d27f532a-41f5-49f3-a140-99afccbac5e4_63603
     host.hostName: <host-name>
     host.mac: 0a:a2:c4:d3:fd:ef
     kubernetes.cluster.name: demo-kube-eks
     kubernetes.deployment.name: voter
     kubernetes.namespace.name: example-voting-app
     kubernetes.node.name: <node-name>
     kubernetes.pod.name: voter-77d98548bc-hmkpc
     kubernetes.replicaSet.name: voter-77d98548bc
   }
   machineId: 0a:a2:c4:d3:fd:ef
   name: Sysdig Runtime Notable Events
   originator: policy
   severity: 4
   source: syscall
   timestamp: 1668293930605536300
   timestampRFC3339Nano: 2022-11-12T22:58:50.60553615Z
   type: policy
}
```

## Add Custom Labels with the Event Labels Attribute

In addition to custom static tags added with the tags attribute in the agent configuration, you can use the `event_labels` attribute to include any label present in the system call. Note the agent will only collect and store events handled by the Sysdig backend.

```yaml
agent.tag.Tag
agent.tag.cluster
agent.tag.environment
agent.tag.role
agent.tag.sysdig_secure.enabled
gcp.projectId
host.hostName
aws.user
container.label.io.kubernetes.pod.namespace
host.mac
kubernetes.statefulSet.name
container.image.repo
container.label.io.kubernetes.pod.name
aws.accountId
gcp.user
gcp.region
container.label.maintainer
kubernetes.service.name
container.image.tag
kubernetes.cluster.name
kubernetes.deployment.name
kubernetes.namespace.label.project
container.label.io.kubernetes.container.name
container.name
kubernetes.pod.name
aws.fargate.task.arn
container.image.digest
kubernetes.daemonSet.name
kubernetes.job.name
kubernetes.namespace.label.field.cattle.io/projectId
kubernetes.namespace.name
kubernetes.node.name
kubernetes.replicaSet.name
aws.region
container.image.id
kubernetes.cronJob.name
aws.fargate.cluster.arn
aws.availabilityZone
```

## Disable Label Collection

Sysdig Agent collects Secure Events labels by default. To disable the feature: disable the feature, change `true` to `false`:

```yaml
event_labels:
  enabled: true
```
