---
linkTitle: "Change the Agent Log Directory"
title: "Change the Agent Log Directory"
weight: "15"
aliases:
  - /en/change-the-agent-log-directory.html
  - /en/change-log-directory.html
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/manage-agent-log-levels/change-the-agent-log-directory/
  - /en/docs/installation/configuration/sysdig-agent/tune-agent/manage-agent-log-levels/change-the-agent-log-directory/
---

The Sysdig agent generates log entries in `/opt/draios/logs/draios.log`.
The agent will rotate the log file when it reaches 10MB in size, keeping
the 10 most recent log files archived with a date-stamp appended to the
filename.

You can change the default location as follows:

```yaml
log:
  location: new_directory
```

By default, this location is rooted in the agent install path: `/opt/draios/`.  Therefore, the new log location for the given example would be `/opt/draios/new_directory`.  

You cannot write agent logs outside of the agent install path.
