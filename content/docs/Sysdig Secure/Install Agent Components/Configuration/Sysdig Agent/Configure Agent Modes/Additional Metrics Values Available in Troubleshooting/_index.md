---
linkTitle: "Additional Metrics Values Available in Troubleshooting"
title: "Additional Metrics Values Available in Troubleshooting"
weight: "11"
aliases:
  - /en/additional-metrics-values-available-in-troubleshooting.html
  - /en/additional-metrics-values-available-in-troubleshooting
  - /en/docs/installation/sysdig-agent/agent-configuration/configure-agent-modes/additional-metrics-values-available-in-troubleshooting/
  - /en/docs/installation/configuration/sysdig-agent/configure-agent-modes/additional-metrics-values-available-in-troubleshooting/
---

In addition to the extensive set of metrics available in the `monitor`
mode, additional metrics, such as `net.sql` and `net.mongodb`, as well
as additional segments for file and network metrics are available.

| Sysdig Legacy ID                     | Prometheus ID                                                | Additional Metrics Values Available When Segmented by |
| ----------------------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| `file.error.total.count`      | `sysdig_host_file_error_total_count`<br>`sysdig_container_file_error_total_count`<br>`sysdig_program_file_error_total_count` | `file.name` and `file.mount` labels                   |
| `file.bytes.total`            | `sysdig_host_file_total_bytes`<br/>`sysdig_container_file_total_bytes`<br/>`sysdig_program_file_total_bytes` |                                                       |
| `file.bytes.in`               | `sysdig_host_file_in_bytes`<br/>`sysdig_container_file_in_bytes`<br/>`sysdig_program_file_in_bytes` |                                                       |
| `file.bytes.out`              | `sysdig_host_file_out_bytes`<br/>`sysdig_container_file_out_bytes`<br/>`sysdig_program_file_out_bytes` |                                                       |
| `file.open.count`             | `sysdig_host_file_open_count`<br>`sysdig_container_file_open_count`<br>`sysdig_program_file_open_count` |                                                       |
| `file.time.total`             |`sysdig_host_file_total_time`<br/>`sysdig_container_file_total_time`<br/>`sysdig_program_file_total_time` |                                                       |
| `host.count`                  | None                                                         |                                                       |
| `host.error.count`            |`sysdig_host_syscall_error_count`<br/>`sysdig_container_syscall_error_count` |                                                       |
| `proc.count`                  |`sysdig_host_proc_count`<br/>`sysdig_container_proc_count`<br/>`sysdig_program_proc_count` |                                                       |
| `proc.start.count`            | None                                                         |                                                       |
| `net.mongodb.collection`      |                                                              | all                                                   |
| `net.mongodb.error.count`     |`sysdig_host_net_mongodb_error_count`<br/>`sysdig_container_net_mongodb_error_count`                                                             |                                                       |
| `net.mongodb.operation`       |                                                              |                                                       |
| `net.mongodb.request.count`   |`sysdig_host_net_mongodb_request_count`<br/>`sysdig_container_net_mongodb_request_count`                                                              |                                                       |
| `net.mongodb.request.time`    |`sysdig_host_net_mongodb_request_time`<br/>`sysdig_container_net_mongodb_request_time`                                                              |                                                       |
| `net.sql.query`               |                                                              | all                                                   |
| `net.sql.error.count`         |`sysdig_host_net_sql_error_count`<br/>`sysdig_container_net_sql_error_count`                                                              |                                                       |
| `net.sql.query.type`          |                                                              |                                                       |
| `net.sql.request.count`       |`sysdig_host_net_sql_request_count`<br/>`sysdig_container_net_sql_request_count`                                                              |                                                       |
| `net.sql.request.time`        |`sysdig_host_net_sql_request_time`<br/>`sysdig_container_net_sql_request_time`                                                              |                                                       |
| `net.sql.table`               |                                                              |                                                       |
| `net.http.error.count`        | `sysdig_host_net_http_error_count`<br/>`sysdig_container_net_http_error_count` | `net.http.url`                                        |
| `net.http.method`             | None                                                         |                                                       |
| `net.http.request.count`      | `sysdig_host_net_http_request_count`<br/>`sysdig_container_net_http_request_count` |                                                       |
| `net.http.request.time`       | `sysdig_host_net_http_request_time`<br>`sysdig_container_net_http_request_time` |                                                       |
| `net.bytes.in`                | `sysdig_host_net_in_bytes`<br/>`sysdig_container_net_in_bytes`<br/>`sysdig_program_net_in_bytes` |                                                       |
| `net.bytes.out`               | `sysdig_host_net_out_bytes`<br/>`sysdig_container_net_out_bytes`<br/>`sysdig_program_net_out_bytes` |                                                       |
| `net.request.time.worst.out`  | None                                                         |                                                       |
| `net.request.count`           | `sysdig_host_net_request_count`<br/>`sysdig_container_net_request_count`<br/>`sysdig_program_net_request_count` |                                                       |
| `net.request.time`            | `sysdig_host_net_request_time`<br/>`sysdig_container_net_request_time`<br/>`sysdig_program_net_request_time` |                                                       |
| `net.bytes.total`             | `sysdig_host_net_total_bytes`<br/>`sysdig_container_net_total_bytes`<br/>`sysdig_program_net_total_bytes`<br/>`sysdig_connection_net_total_bytes` |                                                       |
| `net.http.request.time.worst` |                                                              | all                                                   |

