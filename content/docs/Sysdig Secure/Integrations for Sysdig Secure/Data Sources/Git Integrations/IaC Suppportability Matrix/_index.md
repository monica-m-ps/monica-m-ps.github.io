---
linkTitle: "IaC Supportablility Matrix"
title: "IaC Supportablility Matrix"
weight: "12"
aliases:
  - /en/iac-support.html
  - /en/docs/sysdig-secure/iac-security/iac-suppportability-matrix/
  - /en/iac-support
---

At this time, Sysdig's Infrastructure as Code (IaC) Git-integrated scanning supports the following resource and source types:

| Resource             | Source type    |
| -------------------- | -------------- |
| AWS                  | [Cloud Formation Template (CFT)](https://aws.amazon.com/cloudformation/resources/templates/) |
|                      | [Terraform AWS provider](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |
| Azure                | [Azure Resource Manager (ARM)](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview)|
|                      | [Terraform Azure provider](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)|
| GCP                  | [Terraform Google Cloud Provider](https://registry.terraform.io/providers/hashicorp/google/latest/docs) |
| Kubernetes Workloads | [YAML manifests](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) |
|                      | [Kustomize](https://kustomize.io/) folders |
|                      | [Helm Charts](https://helm.sh/) |
|                      | [Terraform Kubernetes provider](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs) |
