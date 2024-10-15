---
linkTitle: "Disable Captures"
title: "Disable Captures"
weight: "16"
aliases:
  - /en/disable-captures.html
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/disable-captures/
  - /en/docs/installation/sysdig-agent/agent-configuration/filter-data/disable-captures/
---

Sometimes, security requirements dictate that capture functionality should not be triggered at all. For example, PCI compliance for payment information.

In such cases, to disable Captures altogether:

1.  Access the configuration file by using one of the [options listed](/en/configure-sysdig-agent).
    
    This example accesses `dragent.yaml `directly. 
    
2.  Set the parameter:

    ```yaml
    sysdig_capture_enabled: false
    ```

3.  Restart the agent, using the command: 

    ```yaml
    service dragent restart
    ```