---
linkTitle: "Troubleshooting"
title: "Troubleshooting"
weight: "17"
aliases:
  - /en/identity-and-access.html
  - /en/ia-ciem/
  - /en/ciem-trouble
description: "Troubleshoot Identity and access enablement, Sysdig's tool for cloud infrastructure entitlement management (CIEM) ."
---
## Suggestions

### Check read Access

Sysdig’s Identity and Access feature needs `read` access for specific resources such as Identity and Access Management (IAM) to function. Certain Amazon Web Services (AWS) policies block Sysdig from reading data:

- Service control policies (SCP): Ensure there are no restrictions on read access to IAM.
- Certain region level policies that restrict everything to be read from that specific region.

### Check Role Provisioning

Verify the role provisioned for Sysdig is correct with this [API](https://secure.sysdig.com/swagger.html#tag/Cloud-Security/paths/~1api~1cloud~1v2~1accounts~1%7BaccountId%7D~1validateRole/get).

### Check Health of cloud-connector

For AWS, verify the cloud-connector component is healthy by following these steps:

1. In the Sysdig Secure UI, navigate to **Identity** → **Identity Overview**.

2.  Click **Learning Status** in the top right corner. 

   Your connected cloud accounts appear with the status of the cloud-connector and the last time Sysdig received an event listed.

   If cloud-connector is **Disconnected** and the(`Last Event Sent` timestamp is older than a few hours, user activity will not be monitored. Please check logs and contact Sysdig support to help resolve the issue.

##  Limitations

* Currently, only the identity-based policies (managed, inline, and group policies), Organization SCPs, and permission boundaries are considered for permission calculation. Resource-based, Access control lists (ACLs), and Session policies are not yet accounted for during permission calculations.

  * `AWS Last seen time`is based on `GetServiceLastAccessedDetails`. For more information, see [Amazon's documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html#access-advisor_tracking-period). 
  * The AWS permissions used by IAM identities is based on user activity observed in Cloudtrail logs. Currently, permissions used after assuming roles are not taken into account. 