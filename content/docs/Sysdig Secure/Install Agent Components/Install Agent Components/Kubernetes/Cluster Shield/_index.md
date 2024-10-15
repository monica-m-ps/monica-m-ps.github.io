---
title: "Cluster Shield"
weight: "10"
no_list: true
aliases:
  - /en/cluster-shield
  - /en/deploy-cluster-shield
  - /en/docs/installation/sysdig-secure/install-agent-components/kubernetes/cluster-shield/
description: "The Sysdig Cluster Shield streamlines the deployment, management, and configuration of the Sysdig suite of security and compliance tools at the cluster level. By consolidating multiple agent deployments into a single containerized component, Cluster Shield simplifies operations for Kubernetes environments to enable you to maintain the security and compliance posture of your system."
---

These components remain supported individually and deployable as part of the existing `sysdig-deploy` Helm chart.

{{% callout type="note" %}}
You need a Secure API Token to enable the `audit` feature or if you are running an on-premises version older than 6.12.0.
{{% /callout %}}

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

## Migrate to Cluster Shield

If you have previously installed Sysdig components in your cluster, follow the instruction given in this topic to migrate to the Cluster Shield. 

Instructions given in this section are only relevant to the existing users.

The Cluster Shield replaces the following individual components:

- Kubernetes Audit Logging (Admission Controller)

- Secure Admission Controller (KSPM + Vulnerability Management)

- Cluster Scanner (supersedes the Runtime Scanner)

- KSPM Collector

To migrate to the Sysdig Cluster Shield:
1. Disable the components you have already installed by using the `sysdig-deploy` chart.
2. [Install the Sysdig Cluster Shield](#install-cluster-shield).

## Disable the Sysdig Components

If you have any of the following components deployed, disable the following components in the `sysdig-deploy` chart:

- **Kubernetes Audit Logging (Admission Controller)**
- **KSPM Collector**
- **Runtime Scanner**
- **Cluster Scanner**

Add the following configuration to your `values.yaml` or edit your existing installation by using the upgrade command for the `sysdig-deploy` chart.

### Kubernetes Audit Logging (Admission Controller)

```yaml
admissionController:
  enabled: false
```

### Disable KSPM Collector

```yaml
kspmCollector:
  enabled: false
```

### Disable Runtime Scanner

```yaml
nodeAnalyzer:
  nodeAnalyzer:
    runtimeScanner:
      deploy: false
```

### Disable Cluster Scanner

```yaml
clusterScanner:
  enabled: false
```

## Install Cluster Shield

If you are a new user, see  [installation instruction for Kubernetes](/en/install-kubernetes-secure).  

If you have an existing installation of Sysdig Agent you can use the usual command to enable the Cluster Shield and related features:

```bash
helm upgrade .... \
  --set clusterShield.enabled=true \
  --set clusterShield.cluster_shield.features.container_vulnerability_management.enabled=true \
  --set clusterShield.cluster_shield.features.audit.enabled=true \
  --set clusterShield.cluster_shield.features.posture.enabled=true \
  ....
```

Here is the list of features we enable and what they do:
* `container_vulnerability_management`: replaces the *Runtime Scanner* or the *Cluster Scanner* component that you may have enabled
* `audit`: replaces the *Kubernetes Audit Logging (Admission Controller)* component
* `posture`: replaces the *KSPM Collector* component