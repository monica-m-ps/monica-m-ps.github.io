---
linkTitle: "Metrics Not Available in Essentials Mode"
title: "Metrics Not Available in Essentials Mode"
weight: "12"
toc_hide: true
exclude_search: true
hide_summary: true
aliases:
  - /en/metrics-not-available-in-essentials-mode.html
  - /en/docs/installation/sysdig-agent/agent-configuration/configure-agent-modes/metrics-not-available-in-essentials-mode/
  - /en/docs/installation/configuration/sysdig-agent/configure-agent-modes/metrics-not-available-in-essentials-mode/
---

The following metrics will not be reported in the `essentials` mode as compared to the `monitor` mode:

| Sysdig ID                    | Prometheus ID                                                | Segmented By                                                 |
| ---------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `net.bytes.in`               | `sysdig_host_net_in_bytes`<br/>`sysdig_container_net_in_bytes`<br/>`sysdig_program_net_in_bytes` | `net.connection.server`, `net.connection.direction`, `net.connection.l4proto` , and `net.connection.client` labels |
| `net.bytes.out`              | `sysdig_host_net_out_bytes`<br/>`sysdig_container_net_out_bytes`<br/>`sysdig_program_net_out_bytes` |                                                              |
| `net.connection.count.total` | `sysdig_host_net_connection_total_count`<br/>`sysdig_container_net_connection_total_count`<br/>`sysdig_program_net_connection_total_count`<br/>`sysdig_connection_net_connection_total_count` |                                                              |
| `net.connection.count.in`    | `sysdig_host_net_connection_in_count`<br/>`sysdig_container_net_connection_in_count`<br/>`sysdig_program_net_connection_in_count`<br/>`sysdig_connection_net_connection_in_count` |                                                              |
| `net.connection.count.out`   | `sysdig_host_net_connection_out_count`<br/>`sysdig_container_net_connection_out_count`<br/>`sysdig_program_net_connection_out_count`<br/>`sysdig_connection_net_connection_out_count` |                                                              |
| `net.request.count`          | `sysdig_host_net_request_count`<br/>`sysdig_container_net_request_count`<br/>`sysdig_program_net_request_count` |                                                              |
| `net.request.count.in`       | `sysdig_host_net_request_in_count`<br/>`sysdig_container_net_request_in_count`<br/>`sysdig_program_net_request_in_count`<br/>`sysdig_connection_net_request_in_count` |                                                              |
| `net.request.count.out`      | `sysdig_host_net_request_out_count`<br/>`sysdig_container_net_request_out_count`<br/>`sysdig_program_net_request_out_count`<br/>`sysdig_connection_net_request_out_count` |                                                              |
| `net.request.time`           | `sysdig_host_net_request_time`<br/>`sysdig_container_net_request_time`<br/>`sysdig_program_net_request_time` |                                                              |
| `net.request.time.in`        | `sysdig_host_net_time_in_count`<br/>`sysdig_container_net_time_in_count`<br/>`sysdig_program_net_time_out_count`<br/>`sysdig_connection_net_time_in_count` |                                                              |
| `net.request.time.out`       | `sysdig_host_net_time_out_count`<br/>`sysdig_container_net_time_out_count`<br/>`sysdig_program_net_time_out_count`<br/>`sysdig_connection_net_time_out_count` |                                                              |
| `net.bytes.total`            | `sysdig_host_net_total_bytes`<br/>`sysdig_container_net_total_bytes`<br/>`sysdig_program_net_total_bytes`<br/>`sysdig_connection_net_total_bytes` |                                                              |
| `net.mongodb.collection`     |                                                              | all                                                          |
| `net.mongodb.error.count`    | `sysdig_host_net_mongodb_error_count`<br/>`sysdig_container_net_mongodb_error_count` |                                                              |
| `net.mongodb.operation`      |                                                              |                                                              |
| `net.mongodb.request.count`  | `sysdig_host_net_mongodb_request_count`<br/>`sysdig_container_net_mongodb_request_count` |                                                              |
| `net.mongodb.request.time`   | `sysdig_host_net_mongodb_request_time`<br/>`sysdig_container_net_mongodb_request_time` |                                                              |
| `net.sql.query`              |                                                              | all                                                          |
| `net.sql.error.count`        | `sysdig_host_net_sql_error_count`<br/>`sysdig_container_net_sql_error_count` |                                                              |
| `net.sql.query.type`         |                                                              |                                                              |
| `net.sql.request.count`      | `sysdig_host_net_sql_request_count`<br/>`sysdig_container_net_sql_request_count` |                                                              |
| `net.sql.request.time`       | `sysdig_host_net_sql_request_time`<br/>`sysdig_container_net_sql_request_time` |                                                              |
| `net.sql.table`              |                                                              |                                                              |
| `net.sql.query`              |                                                              | all                                                          |
| `net.sql.table`              |                                                              |                                                              |
| `net.http.method`            |                                                              |                                                              |
| `net.http.request.count`     | `sysdig_host_net_http_request_count`<br/>`sysdig_container_net_http_request_count` |                                                              |
| `net.http.request.time`      | `sysdig_host_net_http_request_time`<br/>`sysdig_container_net_http_request_time` |                                                              |
| `net.http.statusCode`        |                                                              |                                                              |
| `net.http.url`               |                                                              |                                                              |

