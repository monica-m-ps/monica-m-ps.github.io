---
linkTitle: "Control Disk Usage by Agent Logs"
title: "Control Disk Usage by Agent Logs"
weight: "17"
aliases:
  - /en/control-the-disk-usage-by-agent-logs.html
  - /en/control-disk-usage-by-agent-logs.html
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/manage-agent-log-levels/control-disk-usage-by-agent-logs/
  - /en/docs/installation/configuration/sysdig-agent/tune-agent/manage-agent-log-levels/control-disk-usage-by-agent-logs/
---

The Sysdig agent generates log entries in `/opt/draios/logs/draios.log`. It periodically  performs rotation of its own logs. 

You can use the following configuration to control the space taken up by agent logs: 

- `max_size`: Sets a limit to the size of a single agent log file, in megabytes. When the log file reaches this size, a new log file will be created. The old log will be renamed with a timestamp. The default size is 10 megabytes.

- `rotate`: The `rotate` configuration determines how many old log files are kept on the disk. The default is 10 log files. 

  When the log file reaches this size, a new log file, `draios.log` will be created, and the old log will be renamed with a timestamp.

```yaml
log:
  max_size: 10
  rotate: 10
```

For example, if the current log file reaches the size limit of 10 megabytes and the number of log files reaches the limit of 10, the oldest will be removed. The last log file will be renamed with a timestamp and added to the list of old log files.

Increasing these values can provide more logs for troubleshooting at the expense of more space.
