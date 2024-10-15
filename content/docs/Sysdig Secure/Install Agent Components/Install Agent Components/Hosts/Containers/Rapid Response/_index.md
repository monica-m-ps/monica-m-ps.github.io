---
linkTitle: "Rapid Response"
title: "Rapid Response"
weight: "13"
no_list: true
aliases:
  - /en/install-container-rapid-response/
description: This page describes how to install Sysdig Rapid Response on non-Kubernetes hosts using containers. With Rapid Response, Sysdig has introduced a way to grant designated Advanced Users in Sysdig Secure the ability to remote connect into a host directly from the Event stream and execute desired commands there.
---

{{% callout type="warning" %}}
Rapid Response team members have access to a full shell from the
Sysdig Secure UI. Responsibility for the security of this powerful
feature rests with you, your enterprise, and your designated employees.
{{% /callout %}}

See also: [Rapid Response](/en/docs/sysdig-secure/threats/respond/).

Note: You can also [install Rapid Response on Kubernetes](/en/install-rapid-response-k8s/).

## Prerequisites

- Retrieve your [access key](/en/agent-access-key/) to use for `SYSDIG_ACCESS_KEY=<your-access-key>`
- Check your Sysdig Secure [endpoint by region](/en/regions-ranges/) to use for `SYSDIG_API_URL=https://<sysdig-url>` (SaaS)
  - This value is custom for on-prem installations.

- Ensure that you have the password used to encrypt all traffic between the user and the host.

{{% callout type="note" %}}
Sysdig cannot recover this password. If lost, you will not be able to start a session, nor will any session logs be recoverable.
{{% /callout %}}

## Installation

1. Run the following Docker command to mount the host directories and binaries to gain access to the host:

   ```
   docker run --hostname $HOST_NAME -d quay.io/sysdig/rapid-response-host-component:latest --endpoint $API_ENDPOINT --access-key $ACCESS_KEY --password $PASSWORD
   ```

   This command will start the Rapid Response Host Component container, passing in environment variables for the hostname, Sysdig API endpoint, access key, and password.

2. To customize the container, you can create a Dockerfile that installs more scripts or command-line utilities. For example, you can add the `kubectl` or `gcloud` command-line utilities by installing them in the Dockerfile.

3. Since the Alpine Linux distribution used by the Sysdig container doesn't support bash by default, you need to install it in the Dockerfile using the `apk` package manager. You can also add any custom scripts or directives at this point.

   ```
   FROM quay.io/sysdig/rapid-response-host-component:latest AS base-image
   
   FROM alpine:3.13
   COPY --from=base-image /usr/bin/host /usr/bin/host
   
   RUN apk update && \
       apk add bash && \
       rm -rf /var/cache/apk/*
   
   # add custom scripts and other directives
   
   ENTRYPOINT ["host"]
   ```

4. Once you've built the custom Docker image, you can use it to start the Sysdig Rapid Response Host Component container. Make sure to set the ENTRYPOINT directive to the command that you want the container to execute when it starts up.

After you install Rapid Response, [configure team(s)](/en/docs/sysdig-secure/threats/respond/#configure-rapid-response-teams) to use it.

## Configure Log Storage

To save session logs, you must configure Rapid Response custom storage.

- If you are using the default storage for Capture files, configure [an AWS or custom S3 bucket](/en/docs/administration/administration-settings/storage-configure-options-for-capture-files/) to store Rapid Response log files after a session. If you have already configured an S3 bucket for Captures, then Rapid Response logs are routed there automatically, into their own folder.

- For SaaS Users with Sysdig Secure Only: Contact Sysdig Support for help to create a custom S3 bucket for rapid response logs.

## Next Steps

[Install the Agent as a container](/en/install-container-agent-secure)

[Install the Host Scanner as a container](/en/install-container-host-scanner)
