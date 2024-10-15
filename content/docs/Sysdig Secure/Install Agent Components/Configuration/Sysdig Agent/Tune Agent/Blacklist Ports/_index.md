---
linkTitle: "Blacklist Ports"
title: "Blacklist Ports"
weight: "19"
aliases:
  - /en/blacklist-ports.html
  - /en/blacklist-ports
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/blacklist-ports/
---

Use the `blacklisted_ports` parameter in the agent configuration file to
block network traffic and metrics from unnecessary network ports.

**Note:** Port 53 (DNS) is always blacklisted.

1.  Access the agent configuration file, using one of the [options listed](/en/configure-sysdig-agent).
    
2.  Add `blacklisted_ports` with desired port numbers.

    **Example (YAML):**

    `blacklisted_ports:`

    ` - 6443`

    ` - 6379`

3.  Restart the agent (if editing `dragent.yaml` file directly), using
    either the `service dragent restart` or
    `docker restart sysdig-agent` command as appropriate.

