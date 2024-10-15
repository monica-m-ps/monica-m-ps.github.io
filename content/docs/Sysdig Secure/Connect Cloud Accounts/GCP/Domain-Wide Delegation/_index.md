---
linkTitle: "Domain-Wide Delegation Permissions"
title: "Domain-Wide Delegation Permissions"
weight: "11"
no_list: true
aliases:
  - /en/gcp-domain-wide-delegation
description: "GCP CIEM product editions fall into two main categories: CIEM Basic and CIEM Advanced. CIEM Advanced facilitates domain-wide delegation, allowing a Google Workspace super administrator to entrust a service account with the capability to gather Google IAM and Workspace resources. This is crucial for conducting routine cloud scans and IAM evaluations. Following is a comparative list of features supported across both editions."
---
## GCP Users                                                                                      

| Permissions               | CIEM Basic                                                   | CIEM Advanced (Domain-Wide Delegation)                       |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Risk Findings             | <ul><li>Editor Role Applied</li><li>Owner Role Applied</li><li>Inactive</li></ul> | <ul><li> No MFA</li><li>Admin</li><li>Editor Role Applied</li><li>Owner Role Applied</li><li>Inactive</li></ul> |
| Profiling Label           | Learning                                                     | Learning                                                     |
| Permissions Calculation   | Available                                                    | Available                                                    |
| Highest Access Evaluation | Available                                                    | Available                                                    |
| Risk Scoring              | Available                                                    | Available                                                    |
| Role Remediations         | Available                                                    | Available                                                    |
| Download CSV Reports      | Available                                                    | Available                                                    |

## GCP Service Accounts

| Permissions               | CIEM Basic                                                   | CIEM Advanced (Domain-Wide Delegation)                       |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Risk Findings             | <ul><li>User Managed Key</li><li>Access Key(s) Not Rotated</li><li>Multiple Access Keys Active</li><li>Owner Role Applied</li><li> Inactive</li><li> Admin</li></ul> | <ul><li>Lateral Movement</li><li>User Managed Key</li><li>Access Key(s) Not Rotated</li><li>Multiple Access Keys Active</li><li>Owner Role Applied</li><li> Inactive</li><li> Admin</li></ul> |
| Profiling Label           | Learning                                                     | Learning                                                     |
| Permissions Calculation   | Available                                                    | Available                                                    |
| Highest Access Evaluation | Available                                                    | Available                                                    |
| Risk Scoring              | Available                                                    | Available                                                    |
| Role Remediations         | Available                                                    | Available                                                    |
| Download CSV Reports      | Available                                                    | Available                                                    |

## GCP Groups

| Permissions               | CIEM Basic                                                   | CIEM Advanced (Domain-Wide Delegation)                       |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Risk Findings             | <ul><li>Editor Role Applied</li><li>Owner Role Applied</li><li>Admin</li></ul> | <ul><li>Inactive</li><li>Admin</li><li>Editor Role Applied</li><li>Owner Role Applied</li></ul> |
| Profiling Label           | Learning                                                     | Learning                                                     |
| Permissions Calculation   | Unavailable                                                  | Available                                                    |
| Highest Access Evaluation | Available                                                    | Available                                                    |
| Risk Scoring              | Unavailable                                                  | Available                                                    |
| Role Remediations         | Unavailable                                                  | Available                                                    |
| Download CSV Reports      | Available                                                    | Available                                                    |

## GCP Roles

| Permissions               | CIEM Basic                           | CIEM Advanced (Domain-Wide Delegation)   |
| ------------------------- | ------------------------------------ | ---------------------------------------- |
| Risk Findings             | <li>Inactive</li><li>Admin</li></ul> | <ul><li>Inactive</li><li>Admin</li></ul> |
| Profiling Label           | Learning                             | Learning                                 |
| Permissions Calculation   | Available                            | Available                                |
| Highest Access Evaluation | Available                            | Available                                |
| Risk Scoring              | Available                            | Available                                |
| Role Remediations         | Available                            | Available                                |
| Download CSV Reports      | Available                            | Available                                |
| Membership Evaluation     | Unavailable                          | Available                                |
