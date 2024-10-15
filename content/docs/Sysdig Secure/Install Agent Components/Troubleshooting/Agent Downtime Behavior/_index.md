---
linkTitle: "Connection between Agent and Sysdig Collector"
title: "Connection between Agent and Sysdig Collector"
weight: "14"
aliases: 
  - /en/connection-between-agent-and-sysdig-collector
Description: "Upon installation, the Sysdig Agent forms a persistent TCP connection with the Sysdig collector in the backend. The agent maintains this connection consistently until either it is restarted or experiences a network disruption. In the event of a disconnection with the Sysdig collector, the agent activates a retry protocol to reinitiate the connection, applying necessary back-off strategies to manage the reconnection attempts effectively."
---
During this period, the Sysdig Agent continues to collect events as they occur, buffers the data locally, and attempts to resend them to the Sysdig collector. To store data locally, the agent maintains an internal buffer of 300 messages. This means that the agent's ability to retain information during a network disruption is entirely dependent on the number of messages generated, not the size of the individual messages.

For example, in a Monitor-only SaaS environment sending metrics every 10 seconds, the agent can queue the data for 50 minutes. This queue far surpasses the Sysdig collector's ability to ingest historical data. If you have a Platform SaaS environment sending a steady stream of metrics, policy events, captures, and so on, the 300 slots could be exhausted quickly, as a single capture can produce far more than 300 messages.

Once the connection is re-established, the agent transmits all queued messages in the order they were generated.

You can configure the queue length by setting the following parameter in the `dragent.yaml` file.

```
collector_endpoint:
    transmit_queue_depth: 300
```

When the queue depth is increased, the additional messages will also be stored in memory, therefore, the memory allocation for the agent will also need to be adjusted in the host. The amount of memory required for the agent is determined by the average size of messages produced by the agent. If the average size of the message is 100 KB and the queue depth is doubled in size from the default 300 to 600, the host requires 100KB*300 = 30 MB additional memory for the agent. But if the average message size is 1MB, the host would require 300 MB more. Most users should not need to adjust the internal queue depth, but if assistance is needed then please contact Sysdig support.
