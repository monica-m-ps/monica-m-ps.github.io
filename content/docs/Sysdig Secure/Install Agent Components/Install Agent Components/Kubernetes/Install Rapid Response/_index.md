---
linkTitle: "Install Rapid Response"
title: "Install Rapid Response"
weight: "10"
no_list: true
aliases:
 - /en/install-rapid-response-k8s/
 - /en/docs/installation/rapid-response-installation/
description: Rapid Response is a feature in Sysdig Secure that allows Advanced Users to remotely access and execute commands on a host from the Event stream. Rapid Response team members have full shell access from within the Sysdig Secure UI.
---

{{% callout type="warning" %}}
Rapid Response team members have access to a full shell from within the
Sysdig Secure UI. Responsibility for the security of this powerful
feature rests with you, your enterprise, and your designated employees.
{{% /callout %}}

See also: [Rapid Response](/en/rapid-response/).

The Rapid Response agent can be installed via Helm on Kubernetes.

**Note:** You can also [install Rapid Response on a host as a container](/en/install-container-rapid-response/).

## Install Using Using Helm

### Prerequisites

- [Installed Sysdig Components on Kubernetes using Helm]( /en/install-kubernetes-secure)
- Helm v3.8 or above

### Deployment Steps

1. Recommended: Replace `helm install` with `helm upgrade --install`.

2. To enable the Rapid Response component in your existing Sysdig Secure Helm install command, add the following:

   ```
   --set rapidResponse.enabled=true \
   --set rapidResponse.rapidResponse.passphrase=YOUR_SECRET_PASSPHRASE \
   ```

   For `passphrase`, enter any phrase you want to use.

3. Run the modified command to deploy Sysdig Secure with the Rapid Response component enabled. [Designated Advanced Users](/en/docs/sysdig-secure/threats/respond/#configure-rapid-response-teams) will now be able to remote connect into a host directly from the Event stream in Sysdig Secure.

## Configure Log Storage

In order to save session logs, Rapid Response requires custom storage to be configured.

If you are using the default storage for Capture files, you will need to configure [an AWS or custom S3 bucket](/en/sysdig-storage/) to store Rapid Response log files after a session. If you have already configured an S3 bucket for Captures, then Rapid Response logs will be routed there automatically, into their own folder.

**For SaaS Users with Sysdig Secure Only**

Contact Sysdig Support for assistance creating a custom S3 bucket for rapid response logs.

## Post-Installation

After Rapid Response is installed, [team(s) must be configured](/en/docs/sysdig-secure/threats/respond/#configure-rapid-response-teams) to use it.

## Troubleshooting

Validate the Rapid Response installation:

- Ensure the host component can reach the Sysdig collector. Its address and port are defined [here](/en/docs/administration/saas-regions-and-ip-ranges/) for SaaS users or provided during the installation for on-prem users.
- Ensure there are no intermediate proxies that could enforce  maximum time to live (since sessions could potentially have long durations)
- Ensure that the host component can reach the object storage (S3 bucket) when configured.
