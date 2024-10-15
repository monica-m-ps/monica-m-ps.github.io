---
linkTitle: "Windows Hosts"
title: "Windows Hosts"
weight: "6"
no_list: true
aliases:
  - /en/docs/installation/windows/
  - /en/install-windows-secure/
  - /en/docs/installation/sysdig-secure/install-agent-components/windows-host/
  
description: The Sysdig Secure Windows Agent provides runtime detection and policy enforcement for host processes on Windows. It uses Falco to ensure workload security and compliance. The agent has two components, the Connection manager and the Security manager, which are both managed by the Agent installer.
---

The Sysdig Windows agent is installed on a Windows host and collects data from that host. It sends the collected data to the Sysdig backend and syncs the Falco runtime policies and rules from the backend to the agent.

## Installation Requirements

### Prerequisites

* Windows Server 2019 and above.
* `ACCESS_KEY`: The [agent access key](/en/agent-access-key).
* `COLLECTOR`: Use the collector address for your region.

  For more information, see [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/).
* Administrator permissions to perform the operations.

## Install the Windows Agent

You can install the Windows Agent using an MSI, which supports both GUI and CLI operation. Download the [MSI package](https://download.sysdig.com/stable/msi/x86_64/agent-windows-1.2.0.msi) from the Sysdig download center.

### GUI Installation

You can execute the MSI using a GUI and the installation process will prompt you to accept the EULA and enter the **Collector** and **Access Key** details.

### CLI Installation

Run the MSI in silent mode via CommandLine or PowerShell:

```powershell
> msiexec /i sysdig-agent.msi COLLECTOR_URL=<COLLECTOR> COLLECTOR_PORT=6443 ACCESS_KEY=<AGENT_ACCESS_KEY> ACCEPT_TERMS_CONDITIONS=True /qn
```

The `COLLECTOR_URL` and `COLLECTOR_PORT` can be omitted if you intend to use the US region as these are assumed by default.

By setting **ACCEPT_TERMS_CONDITIONS** to **True**, you acknowledge and expressly agree that your use of or access to the Sysdig software is governed by the applicable terms and conditions located at [Sysdig Legal Terms](https://sysdig.com/legal/) unless otherwise stated in a Sysdig Order Form or other written mutual agreement between Customer and Sysdig.

## Antivirus/EDR exceptions

Sysdig Windows Agent may conflict when coexisting with Antivirus software or Endpoint Detection and Response (EDR) sensors. To prevent termination of the Sysdig Windows Agent processes, it is recommended to set up exclusions for the Agent root installation directory.

### Carbon Black Cloud

- From the Carbon Black Cloud Console go to Enforce > Policies
- Select the desired Policy and click on the `Prevention` tab
- Add a new `Permission` by clicking on the `+` sign
- Add a new application path in the `Permissions` section and provide the directory exclusion `*:\Program Files\Sysdig\Agent\**`
- Check `Bypass` option box for `Performs Any Operation`
- Click `Confirm`

### Windows Defender

- Open `Windows Security > Virus & threat protection`
- Under `Virus and threat protection settings`, select `Manage Settings`
- Under `Exclusions` select `Add or remove exclusions`
- Click on the `Add an exclusion` button and choose `Folder`
- Browse the drive where the Sysdig Windows Agent was installed, and select the `Program Files\Sysdig\Agent` directory
