---
linkTitle: "Windows on Kubernetes"
title: "Kubernetes on Windows"
weight: "6"
no_list: true
aliases:
  - /en/docs/installation/windows-kubernetes
  - /en/windows-k8s
description: The Sysdig Secure Windows Agent provides runtime detection and policy enforcement for Windows nodes on Kubernetes by leveraging Falco to ensure workload security and compliance. The agent collects data from the Kubernetes node where it is installed. It sends the collected data to the Sysdig backend and syncs the runtime policies and rules from the Sysdig backend to the agent. The agent has two components, the Connection manager and the Security manager, which are both managed by the agent image. 
---

{{% callout type="note" %}}

Supported on Azure Kubernetes Service (AKS). OpenShift support is currently in Technical Preview.

{{% /callout %}}

### Prerequisites

- Windows Server 2019 and above

- `ACCESS_KEY`: The [agent access key](https://github.com/en/agent-access-key)

- `COLLECTOR`: Use the collector address for your region

  For more information, see [SaaS Regions and IP Ranges](https://github.com/en/docs/administration/saas-regions-and-ip-ranges/)

- Administrator permissions to perform the operations
- `Kubectl` installed
- Helm `v3.8` and above


## Use Command Line

### Runtime Threat Detection

To install Sysdig Agent for Runtime Threat Detection and the Cluster Shield for Kubernetes metadata enrichment, use: 



helm repo add sysdig https://charts.sysdig.com
helm install sysdig-agent --namespace sysdig-agent --create-namespace \
    --set global.sysdig.accessKey=<ACCESS_KEY> \
    --set global.sysdig.region=<SAAS_REGION> \
    --set nodeAnalyzer.enabled=false \
    --set global.clusterConfig.name=<CLUSTER_NAME> \
    --set agent.windows.enabled=true \
    --set clusterShield.enabled=true \
    --set clusterShield.cluster_shield.log_level=warn \
    --set clusterShield.cluster_shield.features.kubernetes_metadata.enabled=true \
    sysdig/sysdig-deploy


## Create a values.yaml file with the Following:
global:
  sysdig:
    accessKey: <ACCESS_KEY>
    region: <SAAS_REGION>
  clusterConfig:
    name: <CLUSTER_NAME>
nodeAnalyzer:
  enabled: false
agent:
  windows:
    enabled: true
clusterShield:
  enabled: true
  cluster_shield:
    log_level: warn
    features:
      kubernetes_metadata:
        enabled: true

## Then, install with the following:

helm repo add sysdig https://charts.sysdig.com
helm install -n sysdig-agent sysdig sysdig/sysdig-deploy -f values.sysdig.yaml



### Parameter Definitions

The command above specifies several options:

- `--namespace sysdig-agent`
  - Specifies that the Sysdig deployment should be installed in the "sysdig-agent" namespace.

- `--set global.sysdig.accessKey=<ACCESS_KEY>`
  - Specifies the Sysdig access key to use when connecting to the Sysdig backend. Replace `<ACCESS_KEY>` with your actual access key.

- `--set global.sysdig.region=<SAAS_REGION>`
  - Specifies the Sysdig region to use. Replace `<SAAS_REGION>` with the region where your Sysdig account is located.

- `--set global.clusterConfig.name=<CLUSTER_NAME>`
  - Specifies the name of your Kubernetes cluster. Replace `<CLUSTER_NAME>` with your actual cluster name.

- `--set nodeAnalyzer.enabled=false` 
  - Disables vulnerability management and KSPM features in Sysdig Secure. These are not yet supported on Windows.

- `--set agent.windows.enabled=true`
  - Enables the Windows agent.

- `--set clusterShield.enabled=true`
  - Enables the ClusterShield feature.

- `--set clusterShield.cluster_shield.log_level=warn`
  - Sets the log level for the ClusterShield component to "warn". This means that only warnings and more severe messages will be logged.

- `--set clusterShield.cluster_shield.features.kubernetes_metadata.enabled=true`
  - Enables the Kubernetes metadata feature within the ClusterShield component. This feature provides enhanced metadata for Kubernetes resources.

Once you run the command, the Sysdig Agent will be installed and running on your Kubernetes cluster, starting to send data to the Sysdig backend for analysis and monitoring.

For additional configuration options, see the [**sysdig-deploy readme**](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig-deploy).
