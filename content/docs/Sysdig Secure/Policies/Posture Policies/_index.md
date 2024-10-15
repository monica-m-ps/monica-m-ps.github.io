---
linkTitle: "Posture"
title: "Posture Policies"
weight: "11"
aliases:
  - /en/cspm-policies.html
  - /en/cspm-policies 
  - /en/docs/sysdig-secure/policies/cspm-policies/
  - /en/docs/sysdig-secure/policies/cspm-policies.html
  - /en/posture-policies/
no_list: true
description: "Sysdig Posture Policies allow you to configure what [Compliance](/en/compliance/) evaluates, in the context of compliance standards, such as Center for Internet Security (CIS) and the National Institute of Standards and Technology (NIST). This page provides the conceptual background needed to create, edit, and apply compliance policies in your own environment."
---

Use Posture Policies to: 

* Find policies that meet your organization's needs.
* Create custom policies and configure controls for each requirement.
* Review a policy's structure and the controls connected to it.
* Enable/disable controls on policies.
* Filter controls by enablement status, violation severity, name, and control type.

We add new policies regularly. You can find a comprehensive list of included posture policies on the Posture Policies page in Sysdig Secure.

## Prerequisites

This feature requires the [Compliance](/en/docs/sysdig-secure/posture/compliance/) component.
* [Compliance Installation](/en/docs/sysdig-secure/posture/compliance/#installation)
* [Compliance | Evaluate and Remediate](/en/docs/sysdig-secure/posture/compliance/#evaluate-and-remediate)

## Navigate Policies List 

1. Log in to Sysdig Secure and select  **Policies** > **Posture** | **Policies**.

2. Review the Policies list. Available policies are listed alphabetically. 

   * **Policy Name/Description:** Displays the full policy name and description in accordance with the relevant authority, such as CIS or NIST. Click the arrow to link directly to the relevant standards website, where applicable. 

   * **Zones:** Displays the resource scopes where this policy has been applied. To see results against the policy on the Compliance page, apply a policy to a zone.

   * **Version:** Lists the version of the standard published. It is not to be confused with the version, for example, of Kubernetes, listed in the policy name. 

     ![](/image/cis-version.png)

   * **Date Published:** Date the policy was published. Until officially published, a policy under development is in **Draft** state.

   * **Author:** `Sysdig` for default policies. Email address of creator for custom policies.

3. Click a row to open the individual policy page. 

## Filter Posture Policies

You can search or filter the Posture Policies list by: 

* **Search**: Free-text search for keywords.
* **Zones**: Select zones from the drop-down to filter policies by zones.
* **Published**/**Draft**: Toggle the buttons to filter by status.
  * Custom policies can be in Draft state until the author publishes them. See [Create a Custom Policy](/en/docs/sysdig-secure/policies/posture-policies/manage-posture-policies-and-requirements/#create-a-custom-policy). 