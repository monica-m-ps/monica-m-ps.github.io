---
linkTitle: "Manage Console Logging for Agent Components"
title: "Manage Console Logging for Agent Components"
weight: "12"
aliases:
  - /en/manage-console-logging-for-agent-components.html
  - /en/manage-console-logging-for-agent-components
  - /en/docs/installation/sysdig-agent/agent-configuration/tune-agent/manage-agent-log-levels/manage-console-logging-for-agent-components/
  - /en/docs/installation/sysdig-agent/agent-configuration/manage-agent-log-levels/manage-console-logging-for-agent-components/
  - /en/docs/installation/configuration/sysdig-agent/tune-agent/manage-agent-log-levels/manage-console-logging-for-agent-components/
---

Sysdig Agent provides the ability to set component-wise log levels that
override the global console logging level controlled by the
`console_priority` configuration option. The components represent
internal software modules and can be found in
`/opt/draios/logs/draios.log`.

By controlling logging at the fine-grained component level, you can
avoid excessive logging from certain components in `draios.log` or
enable extra logging from specific components for troubleshooting.

Components can also have an optional feature level logging that
can provide a way to control the logging for a particular feature
in Sysdig Agent.

## Configure Logging

To set feature-level or component-level logging:

1.  Determine the agent component you want to set the log level:

    To do so,

    1.  Look at the console output.

        If you're using an orchestrator like Kubernetes, the log viewer
        facility, such as the `kubectl` log command, shows the console
        log output.

    2.  Copy the component name.

        The format of the log entry is:

        ```yaml
        <timestamp>, <<pid>.<tid>>, <log level>, [feature]:<component>[pid]:[line]: <message>
        ```

        For example, the given snippet from a sample log file shows log
        messages from `promscrape` featture, `sdjagent`, `mountedfs_reader`,
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
          console_priority: notice
          console_priority_by_component:
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
          console_priority: notice
          console_priority_by_component:
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
    
 ## Agent Components

- `analyzer`: The logs from this component provide information about events and metrics as they come into the system. These logs assist in basic troubleshooting of event flow.

- `connection_manager`: This component logs details about the agent's connection to the Sysdig backend. These logs help diagnose and troubleshoot connectivity issues.

- `security_mgr`: These logs describe the security processing steps the agent is taking. Having these logs assists in understanding what the security side of the agent is doing.

- `infrastructure_state`: This component interacts with the orchestration runtime to provide a view of the infrastructure. The logs from this component help troubleshoot orchestration issues and communication with the API server.

- `procfs_parser`: The agent uses the procfs parser to gather information about the state of the system. These logs provide insight into the functioning of the agent.

- `dragent`: These logs provide data about the core functionality of the agent.

- `process_emitter`: This component is used to provide data regarding processes running on a host. 

- `k8s_parser`: The `k8s_parser` is used as part of the communication with the Kubernetes API server. These logs help debug communication issues.

- `netsec`: These logs provide data about the functioning of the `netsec` component, which provides topology and endpoint security functionality.

- `protocol_handler`: This component logs information about the protobufs the agent sends to the Sysdig backend.

- `k8s_deleg`: Kubernetes uses the concept of delegated nodes to help reduce cluster load and manage distributed systems. These logs help with troubleshooting issues within the Kubernetes distributed environment.

- `promscrape`: Promscrape allows the agent to send prometheus data as custom metrics.

- `cm_socket`: The `cm_socket` is the low-level networking code used by the `connection_manager`. These logs work together with the logs from the `connection_manager` to show the behavior of the network connection between the agent and the backend.

- `secure_audit`: Audit is a feature of Sysdig Secure which provides information on system activity such as file and network behavior. These logs help understand the behavior of that feature.

- `memdumper`: The `memdumper` is used to perform back-in-time captures, and logs from this component help troubleshoot any problems which might occur with back-in-time captures.
