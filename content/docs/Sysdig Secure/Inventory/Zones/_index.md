---
linkTitle: "Zones"
title: "Zones"
weight: "5"
aliases:
  - /en/zones.html
  - /en/docs/sysdig-secure/policies/cspm-policies/zones.html
  - /en/docs/sysdig-secure/policies/cspm-policies/zones/
  - /en/docs/sysdig-secure/policies/posture-policies/zones/
  - /en/zones/
  - /en/docs/sysdig-secure/policies/zones/
description: "A zone, in Sysdig, is a collection of scopes that represent important areas of your business. For example, you can create a zone for your production environment, a staging environment, or a region."
---

Sysdig provides two zones by default:

*  **Entire Infrastructure**: This applies to [connected data sources](/en/docs/sysdig-secure/integrations-for-sysdig-secure/data-sources/). Create a new zone to update findings that are reported on the [Compliance](/en/docs/sysdig-secure/posture/compliance/) page. 
*  **Entire Git**: If you have configured [Infrastructure as Code (IaC) scanning with Git integrations](/en/docs/sysdig-secure/iac-security/git-iac-scanning/) to your development pipeline in Git, then the **Entire Git** zone is automatically applied to those source repositories. You can also create more targeted zones for selected Git sources. 


You can create more Zones to suit your organization's needs.

## Create and Configure a Zone

A completed Zone includes:

* Zone name and description
* Zone scope (the area of business to be included)

To create a Zone:

1. Log in to Sysdig Secure, and navigate to **Inventory** > **Zones**.

2. Click **New Zone**.

3. In the **Main Info** part of the Zone definition:

   1. Enter a Zone **Name**

   2. (Optional) Enter Zone **Description**.

4. In the **Scopes** part of the Zone definition you can create scopes.

   1. To create a new scope, click **Add Scope** and select from the available platforms and specify which attributes to include.

5. Click **Save** to save the Zone. The Zone will appear in the list of Zones.

{{% callout type="warning" %}}

Use of the **Region** scope may result in more data being shown to users in the **Posture** > **Identity Management** pages than defined in the Zone.

{{% /callout %}}

### Available Scope Rule Attributes

Supported scope rule attributes vary according to platform:

**AWS**

- Organization
- Account
- Region
- Labels

**Azure**

- Organization
- Subscription
- Region
- Labels

**GCP**

- Organization
- Project
- Region
- Labels

**Host**

- Host Name (for Docker, Linux hosts)
- Cluster
- Agent Tags

**Kubernetes**

- Distribution (AKS, GKE, EKS, Vanilla Kubernetes)
- Cluster name
- Namespace
- Labels
- Agent Tags

**Git**

- Git integration
- Git source(s)

**Image**

- Registry
- Repository

### Use Operators and Values

After the attribute, you can use operators and values.

Sysdig supports two operators:
* `in`: 
  - Matches exact values. Use this to scope specific cluster names. 
  - For example, defining a scope, `Cluster` + `in` + `prd`, will only match the cluster `prd`, if it exists.
  - You can match multiple values. For example, use the scope, `Cluster` + `in` + `prd` + `demo`, to include the clusters `prd` and `demo`.
* `contains`: 
  - Matches a value inside a string. Use this to scope cluster names containing a given value.
  - For example, defining a scope, `Cluster` + `contains` + `prd`, will include clusters such as `myApp-prd`, `prd1`, and `prd-sysdig`.

After the operator, select a value. Each value field has a limitation of 2048 characters per row. For longer values, consider adding scopes. This improves readability and maintenance of your scopes.

Auto-complete values will be based on resources that were scanned and listed in the [Inventory](/en/docs/sysdig-secure/inventory/).
