---
linkTitle: "IaC Policy Controls"
title: "IaC Policy Controls"
weight: "11"
aliases:
  - /en/iac_policy_controls.html
  - /en/docs/sysdig-secure/iac-security/iac-policy-controls/
  - /en/iac-policy-controls
---

## Introduction

Evaluation of IaC resources is performed using the same Posture [policies]( /en/posture-policies/) and [controls](/en/posture-controls) as CSPM.

The set of policies that apply when evaluating a folder in a repository is defined by creating [Zones](/en/docs/sysdig-secure/policies/cspm-policies/zones/).

When running a GitHub integration to check the compliance of a pull request during development, Sysdig will collect all the policies that apply for that context (the repository, folder and branch pattern) according to the defined zones, and run the controls from those polcies that apply for the evaluated resource type.

You can navigate in the product to [**Policies > Posture Policies**](/en/docs/sysdig-secure/policies/posture-policies/) to find the list of requirements and controls for each policy.
