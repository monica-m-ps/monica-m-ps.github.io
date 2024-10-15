---
linkTitle: "Pull Images from Private Registry"
title: "Pull Images from Private Registry"
weight: "14"
aliases:
  - /en/pull-images-from-private-registry
  - /en/docs/installation/configuration/use-private-registry/
description: "You can configure Sysdig Helm charts to pull container images from a private registry that requires authentication."
---

## Prerequisites

Collect the following information associated with your private registry:

- Registry URL
- Username
- Access token or password
- Email address

## Create a Secret for Registry Credentials

Create a Kubernetes secret that stores your private registry credentials.

```
  kubectl create secret docker-registry <SECRET_NAME> \
  --docker-server=<SERVER> \
  --docker-username=<USERNAME> \
  --docker-password=<TOKEN> \
  --docker-email=<YOUR-EMAIL>

```

Replace the placeholders with your registry information.

## Configure Helm Charts

Update the helm installation command or `values.yaml` with the following parameters. You can use either your current one or from the [Kubernetes installation instructions](/en/install-kubernetes-secure/).

Replace the placeholders with your registry information:



helm install ... \

 --set global.imageRegistry=<IMAGE_REGISTRY> \

# Use global pullSecrets and pullPolicy params if they’re shared

 --set 'global.image.pullSecrets[0].name'=<SECRET_NAME> \
 --set global.image.pullPolicy=<PULL_POLICY> \
 --set agent.repository=<IMAGE_REPOSITORY> \
 --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.image.repository=<IMAGE_REPOSITORY> \
 --set nodeAnalyzer.nodeAnalyzer.benchmarkRunner.image.repository=<IMAGE_REPOSITORY> \
 --set nodeAnalyzer.nodeAnalyzer.hostScanner.image.repository=<IMAGE_REPOSITORY> \
 --set nodeAnalyzer.nodeAnalyzer.kspmAnalyzer.image.repository=<IMAGE_REPOSITORY> \
 --set nodeAnalyzer.nodeAnalyzer.imageAnalyzer.image.repository=<IMAGE_REPOSITORY> \
 --set kspmCollector.repository=<IMAGE_REPOSITORY>

# You can use the specific params to override the pullSecrets and pullPolicy if needed for agent

# --set 'agent.image.pullSecrets[0].name'=<SECRET_NAME> \

# --set agent.image.pullPolicy=<PULL_POLICY> \

# for nodeAnalyzer

# --set 'nodeAnalyzer.nodeAnalyzer.pullSecrets[0]'=<SECRET_NAME> \

# --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.image.pullPolicy=<PULL_POLICY> \

# --set nodeAnalyzer.nodeAnalyzer.benchmarkRunner.image.pullPolicy=<PULL_POLICY> \

# --set nodeAnalyzer.nodeAnalyzer.hostScanner.image.pullPolicy=<PULL_POLICY> \

# --set nodeAnalyzer.nodeAnalyzer.kspmAnalyzer.image.pullPolicy=<PULL_POLICY> \

# --set nodeAnalyzer.nodeAnalyzer.imageAnalyzer.image.pullPolicy=<PULL_POLICY> \

# for kspmCollector

# --set 'kspmCollector.imagePullSecrets[0].name'=<SECRET_NAME>

# --set kspmCollector.image.pullPolicy=<PULL_POLICY>



global:
  imageRegistry: <IMAGE_REGISTRY>

# Optional shared image pullSecrets and pullPolicy

  image:
    pullSecrets:
      - name: <SECRET_NAME>
    pullPolicy: <PULL_POLICY>

# This pulls the agent image from the private repository

agent:
  image:
    # Specific pullSecrets and pullPolicy override for agent
    # pullSecrets
    # - name: <SECRET_NAME>
    # pullPolicy: <PULL_POLICY>
    repository: <IMAGE_REPOSITORY>

# This pulls the nodeAnalyzer images from the private repository

nodeAnalyzer:

# Specific pullSecrets override for nodeAnalyzer

# nodeAnalyzer

# pullSecrets

# - name: <SECRET_NAME>

  hostScanner:
    image:
      repository: <IMAGE_REPOSITORY>
      # Specific pullPolicy override for hostScanner
      # pullPolicy: <PULL_POLICY>
  runtimeScanner:
    image:
      repository: <IMAGE_REPOSITORY>
      # Specific pullPolicy override for runtimeScanner
      # pullPolicy: <PULL_POLICY>
  kspmAnalyzer:
    image:
      repository: <IMAGE_REPOSITORY>
      # Specific pullPolicy override for kspmAnalyzer
      # pullPolicy: <PULL_POLICY>

# This pulls the KSPM collector image from a private repository

kspmCollector:
  repository: <IMAGE_REPOSITORY>

# Specific pullSecrets and pullPolicy override

# imagePullSecrets

# - name: <SECRET_NAME>

# image

# pullPolicy: <PULL_POLICY>


