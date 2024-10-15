---
linkTitle: "Managed Kubernetes"
title: "Managed Kubernetes"
weight: "20"
aliases:
  - /en/managed-kubernetes.html
  - /en/managed-kubernetes/
description: "The Managed Kubernetes tab displays the list of managed Kubernetes clusters (GKE, AKS, EKS) detected among the full set of connected cloud accounts in your environment."
---

### Review Managed Kubernetes

Here you can review details and instrument a cluster if needed. 

![](/image/ds_mk2.png)

#### Filtering Actions 

You can: 

* **Search** by keyword
* **Filter** by platform or account number 
* **Sort** by Status, Cluster Name, Account ID, or Region 

#### Use Instrumentation Modal 

For un-instrumented clusters detected on an account, the modal under `More` helps speed the instrumentation process. 

1. Click **Instructions to Instrument**.  

   The instrumentation popup appears with your access key and cluster-specific data prefilled. 

   ![](/image/ds_instrument3.png)

2. Follow the instructions in the wizard using Helm or `values.yaml`.

