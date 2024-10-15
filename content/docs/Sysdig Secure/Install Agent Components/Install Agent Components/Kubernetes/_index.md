---
linkTitle: "Linux on Kubernetes"
title: "Linux on Kubernetes"
weight: "2"
no_list: true
aliases:
- /en/docs/installation/node-analyzer-multi-feature-installation/
- /en/docs/installation/sysdig-agent/agent-installation/agent-install-kubernetes/
- /en/docs/installation/kspm-components/
- /en/install-kubernetes-secure/
- /en/install-kubernetes-secure
- /en/agent-install-kubernetes.html
- /en/kubernetes-agent-installation-steps.html
- /en/on-premises-upgrades.html
- /en/docs/installation/sysdig-secure/install-agent-components/kubernetes/
description: "Sysdig provides a variety of deployment options for the Sysdig agent and associated components, including standalone installations and container deployments. However, for users deploying Sysdig components on Kubernetes, only installation using the Helm package manager is supported. If you are just installing agents for Sysdig Monitor, see [Sysdig Monitor Installation](/en/install-kubernetes-monitor). If you are installing agents for an on-prem deployment, see [On-Premises Agent Installation](/en/onprem-sysdig-agent)."
---

The Sysdig Helm chart [sysdig-deploy](https://charts.sysdig.com/charts/sysdig-deploy/) includes configuration options for customizing the agent's behavior and integrating with other Sysdig components. By using the Helm chart, you can easily deploy the Sysdig Agent on Kubernetes and take advantage of Sysdig's powerful monitoring and security capabilities.

## Prerequisites

- `Kubectl` installed
- Helm `v3.8` and above
- Your [agent access key](/en/agent-access-key/) 
-  [Sysdig SaaS region](/en/regions-ranges/).

## Use the Quick Start Wizard

1. Log in to Sysdig Secure as an administrator and select **Integrations > Data Sources|Sysdig Agent**.

2. Click **+Add Agent** and select **Kubernetes**. The **Helm** installation method is recommended. 

3. As prompted by the Wizard screen, enter:

   * **Cluster Name:** Specify the name of your Kubernetes cluster.

4. The Wizard will auto-populate a code snippet from the cluster name, along with autodetected Sysdig Secure endpoint and [agent access key](/en/agent-access-key/) information.

5. Copy and run the Helm commands. This will install the Cluster Shield and give you: 

   * **Runtime Threat Detection**
   * **Vulnerability Host Scanning**
   * **Runtime Image Scanning**
   * **Kubernetes Security Posture Management (Compliance)**

Otherwise, you can use the generic Helm commands detailed below and customize as needed. 

## Use Command Line

### Full Deploy

This command replicates the Wizard process and installs the Sysdig Cluster Shield and the additional components to deliver runtime threat detection, host scanning, runtime image scanning, and compliance, or Kubernetes Security Posture Management (KSPM). 

It uses the Helm chart `sysdig-deploy`. 




helm repo add sysdig https://charts.sysdig.com
helm repo update
helm install -n sysdig-agent --create-namespace \
  --set global.clusterConfig.name=<CLUSTER_NAME> \
  --set global.sysdig.region=<SAAS_REGION> \
  --set global.sysdig.accessKey=<ACCESS_KEY> \
  --set global.kspm.deploy=true \
  --set nodeAnalyzer.enabled=true \
  --set nodeAnalyzer.nodeAnalyzer.benchmarkRunner.deploy=false \
  --set nodeAnalyzer.nodeAnalyzer.hostAnalyzer.deploy=false \
  --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.deploy=false \
  --set nodeAnalyzer.nodeAnalyzer.imageAnalyzer.deploy=false \
  --set nodeAnalyzer.nodeAnalyzer.hostScanner.deploy=true \
  --set kspmCollector.enabled=false \
  --set admissionController.enabled=false \
  --set clusterShield.enabled=true \
  --set clusterShield.cluster_shield.log_level=warn \
  --set clusterShield.cluster_shield.features.audit.enabled=true \
  --set clusterShield.cluster_shield.features.container_vulnerability_management.enabled=true \
  --set clusterShield.cluster_shield.features.posture.enabled=true \
sysdig sysdig/sysdig-deploy



## Create a values.sysdig.yaml file with the following:
global:
  clusterConfig:
    name: <CLUSTER_NAME>
  sysdig:
    region: <SAAS_REGION>
    accessKey: <ACCESS_KEY>
  kspm:
    deploy: true
nodeAnalyzer:
  enabled: true
  nodeAnalyzer:
    benchmarkRunner:
      deploy: false
    hostAnalyzer:
      deploy: false
    runtimeScanner:
      deploy: false
    imageAnalyzer:
      deploy: false
    hostScanner:
      deploy: true
kspmCollector:
  enabled: false
admissionController:
  enabled: false
clusterShield:
  enabled: true
  cluster_shield:
    log_level: warn
    features:
      audit:
        enabled: true
      container_vulnerability_management:
        enabled: true
      posture:
        enabled: true

## Deploy the Chart

  helm repo add sysdig https://charts.sysdig.com
  helm install -n sysdig-agent sysdig sysdig/sysdig-deploy -f values.sysdig.yaml



### Runtime Threat Detection Only

To install only Runtime Threat Detection (Sysdig Agent), use: 



helm repo add sysdig https://charts.sysdig.com
helm install sysdig-agent --namespace sysdig-agent --create-namespace \
    --set global.sysdig.accessKey=<ACCESS_KEY> \
    --set global.sysdig.region=<SAAS_REGION> \
    --set nodeAnalyzer.enabled=false \
    --set global.clusterConfig.name=<CLUSTER_NAME> \
    sysdig/sysdig-deploy
    


## Create a values.sysdig.yaml file with the Following:
global:
  sysdig:
    accessKey: <ACCESS_KEY>
    region: <SAAS_REGION>
  clusterConfig:
    name: <CLUSTER_NAME>
nodeAnalyzer:
  enabled: false

## Then, install with the following:

helm repo add sysdig https://charts.sysdig.com
helm install -n sysdig-agent sysdig sysdig/sysdig-deploy -f values.sysdig.yaml



### Pod Security Admission

If you’re enforcing PSA, add the `privileged` policy to the sysdig-agent namespace:

```bash
kubectl label --overwrite ns sysdig-agent pod-security.kubernetes.io/enforce=privileged
```

### Parameter Definitions

The command above specifies several options:

- `--namespace sysdig-agent`
  - Specifies that the Sysdig deployment should be installed in the "sysdig-agent" namespace.

- `--set global.sysdig.accessKey=<ACCESS_KEY>`
  - Specifies the Sysdig access key to use when connecting to the Sysdig backend. Replace `<ACCESS_KEY>` with your actual access key.

- `--set global.sysdig.region=<SAAS_REGION>`
  - Specifies the Sysdig region to use. Replace `<SAAS_REGION>` with the region where your Sysdig account is located.

- `--set nodeAnalyzer.secure.vulnerabilityManagement.newEngineOnly=true` 
  - Enables the new engine for vulnerability management in Sysdig Secure.

- `--set global.kspm.deploy=true`
  - Enables the deployment of the KSPM Collector and Analyzer components.

- `--set nodeAnalyzer.nodeAnalyzer.benchmarkRunner.deploy=false`
  - Disables the deployment of the legacy Node Analyzer Benchmark Runner component.

- `--set nodeAnalyzer.nodeAnalyzer.hostScanner.deploy=true`
  - Installs the Host Scanner.

- `--set global.clusterConfig.name=<CLUSTER_NAME>`
  - Specifies the name of your Kubernetes cluster. Replace `<CLUSTER_NAME>` with your actual cluster name.

- `--set global.sysdig.tags.<myKey>=<myValue>`
  - Optionally, add user-defined tag by defining a Key:Value pair. Replace `<myKey>` with your tag key and `<myValue>` with your tag value. you could add multiple tags by adding another row per tag.

After running these commands, the Sysdig Cluster Shield and associated components should be installed and running on your Kubernetes cluster, and will begin sending data to the Sysdig backend for analysis and monitoring.

#### About Host Scanner 

The runtime scanner and host scanner are deployed by default with the given Helm commands. 

##### Opting Out 

If for some reason you don't want to use host scanning, you can opt-out using the Helm chart flag: 

```
--set nodeAnalyzer.nodeAnalyzer.hostScanner.deploy=false
```

### Specific Kubernetes Platforms

#### OpenShift, GKE, OKE, and MKE

If you are using Openshift, Google Kubernetes Engine (GKE) Standard, Oracle Kubernetes Engine (OKE), or Mirantis Kubernetes engine (MKE), enable eBPF with the following option:

- `--set agent.ebpf.enabled=true`
  
#### GKE Autopilot  

If you are using **GKE autopilot**, enable the following option:

- `-–set agent.gke.autopilot=true`

#### Rancher Kubernetes Engine 1

Rancher Kubernetes Engine V1 (RKE1) internally changes container names at the Docker layer in Kubernetes. For instance, while a container may be named `nginx` in your cluster, its underlying name in the Docker engine might resemble `k8s_nginx_nginx_test-container-id-b277-3066f0a5-7115-48f0-bb2d-c48f71663087_9d80f7b5-1827-4a05-83`. The Sysdig Agent communicates with the Docker engine to retrieve container names, and the internal names will be displayed in the Sysdig UI instead of the container name provided within the cluster.

#### Rancher Kubernetes Engine 2

Rancher Kubernetes Engine V2(RKE2) uses a different containerd socket path, `/run/k3s/containerd/containerd.sock`, instead of the expected `/var/run/containerd/containerd.sock`. To accommodate this, ensure that you mount the alternative path into the runtime scanner via the chart's values. This is necessary for the scanner to connect to the containerd socket and identify running containers for subsequent image scanning. Update the chart's values accordingly.

##### Using the Key-Value Pair
```
--set nodeAnalyzer.nodeAnalyzer.extraVolumes.volumes[0].name=socketpath \
--set nodeAnalyzer.nodeAnalyzer.extraVolumes.volumes[0].hostPath.path=/run/k3s/containerd/containerd.sock \
--set nodeAnalyzer.nodeAnalyzer.runtimeScanner.extraMounts[0].name=socketpath \
--set nodeAnalyzer.nodeAnalyzer.runtimeScanner.extraMounts[0].mountPath=/var/run/containerd/containerd.sock \
```

##### Using values.sysdig.yaml
```
nodeAnalyzer:
  nodeAnalyzer:
    extraVolumes:
      volumes:
        - name: socketpath
          hostPath:
            path: /run/k3s/containerd/containerd.sock
    runtimeScanner:
      extraMounts:
        - name: socketpath
          mountPath: /var/run/containerd/containerd.sock
```

#### Mirantis Kubernetes Engine

The Mirantis Kubernetes Engine uses Docker as its runtime, so to accommodate this, make sure you mount the Docker application path in the Agent via the chart's values. This is necessary in order for the Agent to have malware detection and malware prevention to work properly. Update the chart's values accordingly.

##### Using the Key-Value Pair
```
--set agent.extraVolumes.volumes[0].name=sysdig-docker-apps \
--set agent.extraVolumes.volumes[0].hostPath.path=/opt/apps/docker \
--set agent.extraVolumes.mounts[0].mountPath=/host/opt/apps/docker \
--set agent.extraVolumes.mounts[0].name=sysdig-docker-apps \
--set agent.extraVolumes.mounts[0].readOnly=true
```

##### Using values.sysdig.yaml
```
agent:
  extraVolumes:
    volumes:
      - name: sysdig-docker-apps
        hostPath:
          path: /opt/apps/docker
    mounts:
      - mountPath: /host/opt/apps/docker
        name: sysdig-docker-apps
        readOnly: true
```

#### Additional Volumes

Additional volumes might be required in the Sysdig Agent for mapping ConfigMaps, Secrets or additional host paths.
In some specific cases, such as SLES15 on Rancher, the proper `ld-linux-*` library is under the host `/lib64` so the kernel module build fails. To handle, add a specific volume mount `/lib64` to `/host/usr/lib64`. For example:

```yaml
agent:
  daemonset:
    kmodule:
      extraVolumes:
        volumes:
          - name: lib64-vol
            hostPath:
              path: /lib64
        mounts:
          - mountPath: /host/usr/lib64
            name: lib64-vol
```

While for the main Agent container you can specify the `extraVolumes` in the agent root like:

```yaml
agent:
  extraVolumes:
    volumes:
      - name: repo-new-cm
        configMap:
          name: my-cm
          optional: true
      - name: repo-new-secret
          secret:
          secretName: my-secret
    mounts:
      - mountPath: mount-path
        name: repo-new-cm
      - mountPath: mount-path
        name: repo-new-secret
```

For additional configuration options, including on-premises or using a proxy, see the [**sysdig-deploy readme**](https://github.com/sysdiglabs/charts/tree/master/charts/sysdig-deploy).

You can also use Helm to 

Install [Kubernetes Audit Logging](/en/install-audit-logging) (Admission Controller)

Install [Rapid Response](/en/install-rapid-response-k8s/)
