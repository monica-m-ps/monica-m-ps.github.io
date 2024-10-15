---
title: "Host Shield (Technical Preview)"
weight: "10"
no_list: true
aliases:
  - /en/host-shield
  - /en/deploy-host-shield
description: "The Sysdig Host Shield streamlines the deployment, management, and configuration of the Sysdig suite of security and compliance tools at the host level. By consolidating multiple agent deployments into a single containerized component, Host Shield simplifies operations for Kubernetes environments to enable you to maintain the security and compliance posture of your system."
---

These components remain supported individually and deployable as part of the existing `sysdig-deploy` Helm chart for now. See [Sysdig Deploy](https://charts.sysdig.com/charts/sysdig-deploy/).

## Prerequisites

- `kubectl` installed
- Helm `v3.8` and above
- Your [agent access key](/en/agent-access-key/) 
- Sysdig Secure **Endpoint** four your [Sysdig SaaS region](/en/regions-ranges/)

## Benefits

#### Simplified Installation and Upgrades

- **Unified installation process**: A single artifact for installation simplifies Sysdig onboarding.
- **Streamlined upgrade paths**: Reduced complexity and consistency across environments through simplified upgrade processes.

#### Unified Versioning

- **Single source**: Centralized versioning information and release notes for the consolidated components.
- **Easier tracking**: Simplified monitoring of new features, defect fixes, and performance enhancements.

#### Improved Compatibility and Support

- **Enhanced compatibility**: Improved support across Sysdig suite of tools.
- **Streamlined support process**: A unified agent approach simplifies troubleshooting and resolution efforts.

## Transition from Sysdig Agent to Host Shield

As Sysdig continues to evolve, the Sysdig Agent will transition fully to being referred to as the **Host Shield**, though the name of the container, RPM, and debs have not changed. This rebranding reflects the integrated nature of the tool and its expanded role in providing host-level security and compliance.

## Migrate to Host Shield

If you have previously installed Sysdig components in your cluster or are considering a fresh installation, use the following instruction to install or migrate to the Host Shield. 

### Components Replaced by Host Shield

The Host Shield replaces the following individual agent components:

- **Sysdig Agent (Runtime Threat Detection)**
- **Host Scanner (Host Vulnerability Scanning)**
- **KSPM Analyzer (Host Posture)**
- **Rapid Response**

By migrating to Host Shield, you consolidate these components into a single, easier-to-manage solution, enhancing the security and compliance posture of your system while simplifying management and support.

### Install Using Helm

To install Host Shield and Cluster Shield, you can use the following `helm install` command with either configuration parameters or a custom `values.yaml` file. 


helm repo add sysdig https://charts.sysdig.com
helm repo update
helm install -n sysdig-agent --create-namespace \
  --set global.clusterConfig.name=<CLUSTER_NAME> \
  --set global.sysdig.region=<SAAS_REGION> \
  --set global.sysdig.accessKey=<ACCESS_KEY> \
  --set global.kspm.deploy=false \
  --set nodeAnalyzer.enabled=false \
  --set clusterShield.enabled=true \
  --set clusterShield.cluster_shield.features.audit.enabled=true \
  --set clusterShield.cluster_shield.features.container_vulnerability_management.enabled=true \
  --set clusterShield.cluster_shield.features.posture.enabled=true \
  --set agent.sysdig.settings.sysdig_api_endpoint=<API_ENDPOINT> \
  --set agent.sysdig.settings.host_scanner.enabled=true \
  --set agent.sysdig.settings.kspm_analyzer.enabled=true \
  --set agent.extraVolumes.volumes[0].name=root-vol \
  --set agent.extraVolumes.volumes[0].hostPath.path=/ \
  --set agent.extraVolumes.volumes[1].name=tmp-vol \
  --set agent.extraVolumes.volumes[1].hostPath.path=/tmp \
  --set agent.extraVolumes.mounts[0].mountPath=/host \
  --set agent.extraVolumes.mounts[0].name=root-vol \
  --set agent.extraVolumes.mounts[0].readOnly=true \
  --set agent.extraVolumes.mounts[1].mountPath=/host/tmp \
  --set agent.extraVolumes.mounts[1].name=tmp-vol \

sysdig sysdig/sysdig-deploy


global:
  clusterConfig:
    name: <CLUSTER_NAME>
  sysdig:
    region: <SAAS_REGION>
    accessKey: <ACCESS_KEY>
  kspm:
    deploy: false

agent:
  sysdig:
    settings:
      sysdig_api_endpoint: <API_ENDPOINT>
      host_scanner:
        enabled: true
      kspm_analyzer:
        enabled: true
  # extraVolumes and mounts sections are needed for the host-scanner and kspm-analyzer now within the agent
  extraVolumes:
    volumes:
    - name: root-vol
      hostPath:
        path: /
    - name: tmp-vol
      hostPath:
        path: /tmp
    mounts:
    - mountPath: /host
      name: root-vol
      readOnly: true
    # The host's tmp directory needs to be writeable for kspm-analyzer to create the scripts it runs.
    - mountPath: /host/tmp
      name: tmp-vol

nodeAnalyzer:
  enabled: false

admissionController:
  enabled: false

clusterShield:
  enabled: true
  cluster_shield:
    features:
      audit:
        enabled: true
      container_vulnerability_management:
        enabled: true
      posture:
        enabled: true


{{% callout type="note" %}}

Google Kubernetes Engine (GKE) Autopilot is not supported in this Technical Preview. 

{{% /callout %}}

### Enable Rapid Response

To enable Rapid Response, add the following to the Helm install command or to the Sysdig settings in the `values.yaml` file:


  --set agent.sysdig.settings.rapid_response.enabled=true \
  --set agent.sysdig.settings.rapid_response.password=<rapid-response-password>

agent:
  sysdig:
    settings:
      rapid_response:
        enabled: true
        password: <rapid-response-password>


