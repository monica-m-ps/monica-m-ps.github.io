---
linkTitle: "Understand the Windows Agent Configuration"
title: "Understand the Windows Agent Configuration"
weight: "10"
aliases:
  - /en/understand-the-windows-agent-configuration.html
  - /en/understand-the-windows-agent-configuration
  - /en/docs/installation/sysdig-windows-agent/agent-configuration/understand-the-agent-configuration/
---

The agent relies on a configuration file named `dragent.yaml` to describe the configuration that influences the behaviour of the Windows Agent. This file is located in the `%ProgramFiles%\Sysdig\Agent\Config` directory.  You can add configuration parameters directly in YAML as key-value pairs.

## Environments 

### Hosts

If Sysdig Windows Agent is installed in a Windows host, edit the `dragent.yaml` file directly.

## Edit the Configuration File

### dragent.yaml 

1. Log in to the host where the agent is installed.

2. Open `%ProgramFiles%\Sysdig\Agent\Config\dragent.yaml`.

3. Edit the file using proper [YAML syntax](https://en.wikipedia.org/wiki/YAML#Syntax).

4. Restart the agent services for changes to take effect.


```powershell
> Restart-Service -DisplayName "Sysdig Connection Manager"
> Restart-Service -DisplayName "Sysdig Security Manager"
```
