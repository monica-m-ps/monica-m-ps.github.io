---
linkTitle: "Cluster Shield Troubleshooting"
title: "Cluster Shield Troubleshooting"
weight: "10"
aliases:
  - /en/cluster-shield-troubleshooting.html
  - /en/cluster-shield-troubleshooting
  - /en/docs/installation/sysdig-cluster-shield/cluster-shield-troubleshooting/
---

## CNI on EKS

When using a custom CNI on EKS, the API server will not be able to reach the webhook endpoint. This happens because the control plane cannot be configured to run on a custom CNI on EKS.
In order to resolve this, when installing Cluster Shield via Helm, apply the following configurations:

```yaml
clusterShield:
  hostNetwork: true
  features:
    audit:
      http_port: 5000 # Required to avoid conflicts
    admission_control:
      http_port: 6000 # Required to avoid conflicts
```

Update the inbound rule in the EKS worker nodes security group, allowing TCP communication on port `5000` and `6000` from the EKS cluster security group.
