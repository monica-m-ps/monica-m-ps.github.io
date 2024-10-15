---
linkTitle: "Manage File Logging for Agent Components"
title: "Manage File Logging for Agent Components"
weight: "11"
aliases:
  - /en/manage-file-logging-for-agent-components.html
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/manage-agent-log-levels/manage-file-logging-for-agent-components/
  - /en/docs/installation/sysdig-agent/agent-configuration/manage-agent-log-levels/manage-file-logging-for-agent-components/
  - /en/docs/installation/configuration/sysdig-agent/tune-agent/manage-agent-log-levels/manage-file-logging-for-agent-components/
---

Sysdig Agent provides the ability to set component-wise log levels that
override the global file logging level controlled by the `file_priority`
configuration option. The components represent internal software modules
and can be found in `/opt/draios/logs/draios.log`.

By controlling logging at the fine-grained component level, you can
avoid excessive logging from certain components in `draios.log` or
enable extra logging from specific components for troubleshooting.

The [Agent components](/en/manage-console-logging-for-agent-components) can also have an optional feature level logging that
can provide a way to control the logging for a particular feature
in Sysdig Agent.

To set feature-level or component-level logging:

1.  Determine the agent feature or component you want to set the log level:

    To do so,

    1.  Open the `/opt/draios/logs/draios.log` file.

    2.  Copy the component name.

        The format of the log entry is:

        ```yaml
        <timestamp>, <<pid>.<tid>>, <log level>, [feature]:<component>[pid]:[line]: <message>
        ```

        For example, the given snippet from a sample log file shows log
        messages from `promscrape` feature, `sdjagent`, `mountedfs_reader`,
        `watchdog_runnable`, `protobuf_file_emitter`,
        `connection_manager`, and `dragent`.

        ```yaml
        2020-09-07 17:56:01.173, 27979.28018, Information, sdjagent[27980]: Java classpath: /opt/draios/share/sdjagent.jar
        2020-09-07 17:56:01.173, 27979.28018, Information, mountedfs_reader: Starting mounted_fs_reader with pid 27984
        2020-09-07 17:56:01.174, 27979.28019, Information, watchdog_runnable:105: connection_manager starting
        2020-09-07 17:56:01.174, 27979.28019, Information, protobuf_file_emitter:64: Will save protobufs for all message types
        2020-09-07 17:56:01.174, 27979.28019, Information, connection_manager:282: Initiating connection to collector
        2020-09-07 17:56:01.175, 27979.27979, Information, dragent:1243: Created Sysdig inspector
        2020-09-07 18:52:40.065, 27979.27980, Debug,       promscrape:prom_emitter:72: Sent 927 Prometheus metrics of 7297 total
        2020-09-07 18:52:41.129, 27979.27981, Information, promscrape:prom_stats:45: Prometheus timeseries statistics, 5 endpoints
        ```
2.  To set feature-level logging: 
    1.  Open `/opt/draios/etc/dragent.yaml`.

    2.  Edit the `dragent.yaml` file and add the desired feature:

        In this example, you are setting the global level to notice and
        `promscrape` feature level to info.

        ```yaml
        log:
          file_priority: notice
          file_priority_by_component:
            - "promscrape: info"
        ```

        The log levels specified for feature override global settings.

3.  To set component-level logging: 
    1.  Open `/opt/draios/etc/dragent.yaml`.

    2.  Edit the `dragent.yaml` file and add the desired feature:

        In this example, you are setting the global level to notice and
        `promscrape` feature level to info, `sdjagent`, `mountedfs_reader`
        component log level to debug, `watchdog_runnable` component log level
        to warning and `promscrape:prom_emitter` component log level to debug.

        ```yaml
        log:
          file_priority: notice
          file_priority_by_component:
            - "promscrape: info"
            - "promscrape:prom_emitter: debug"
            - "watchdog_runnable: warning"
            - "sdjagent: debug"
            - "mountedfs_reader: debug" 
        ```

        The log levels specified for feature override global settings.
        The log levels specified for component overide feature and global settings.

4.  Restart the agent.

    For example, if you have installed the agent as a service, then run:

    ```yaml
    $ service dragent restart

