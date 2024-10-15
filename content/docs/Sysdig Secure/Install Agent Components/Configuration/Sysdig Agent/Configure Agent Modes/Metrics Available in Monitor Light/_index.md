---
linkTitle: "Metrics Available in Monitor Light"
title: "Metrics Available in Monitor Light"
weight: "10"
aliases:
  - /en/metrics-available-in-monitor-light.html
  - /en/metrics-available-in-monitor-light
  - /en/docs/installation/sysdig-agent/agent-configuration/configure-agent-modes/metrics-available-in-monitor-light/
---

Monitor Light provides cpu, memory, file, file system, and network metrics.

| Sysdig Legacy ID                     | Prometheus ID                                                |
| ----------------------------- | ------------------------------------------------------------ |
| `cpu.cores.used`              | `sysdig_host_cpu_cores_used`<br/>`sysdig_container_cpu_cores_used`<br/>`sysdig_program_cpu_cores_used` |
| `cpu.cores.used.percent`      | `sysdig_host_cpu_cores_used_percent`<br/>`sysdig_container_cpu_cores_used_percent`<br/>`sysdig_program_cpu_cores_used_percent` |
| `cpu.idle.percent`            | `sysdig_host_cpu_idle_percent`                               |
| `cpu.iowait.percent`          | `sysdig_host_cpu_iowait_percent`                             |
| `cpu.nice.percent`            | `sysdig_host_cpu_nice_percent`                               |
| `cpu.stolen.percent`          | `sysdig_host_cpu_stolen_percent`                             |
| `cpu.system.percent`          | `sysdig_host_cpu_system_percent`                             |
| `cpu.used.percent`            | `sysdig_host_cpu_used_percent`<br/>`sysdig_container_cpu_used_percent`<br/>`sysdig_program_cpu_used_percent` |
| `cpu.user.percent`            | `sysdig_host_cpu_user_percent`                               |
| `load.average.percpu.1m`      | `sysdig_host_load_average_percpu_1m`                         |
| `load.average.percpu.5m`      | `sysdig_host_load_average_percpu_5m`                         |
| `load.average.percpu.15m`     | `sysdig_host_load_average_percpu_15m`                        |
| `memory.bytes.available`      | `sysdig_host_memory_available_bytes`                         |
| `memory.bytes.total`          | `sysdig_host_memory_total_bytes`                             |
| `memory.bytes.used`           | `sysdig_host_memory_used_bytes`<br/>`sysdig_container_memory_used_bytes`<br/>`sysdig_program_memory_used_bytes` |
| `memory.bytes.virtual`        | `sysdig_host_memory_virtual_bytes`<br/>`sysdig_container_memory_virtual_bytes`<br/>`sysdig_program_memory_virtual_bytes` |
| `memory.pageFault.major`      | None                                                         |
| `memory.pageFault.minor`      | None                                                         |
| `memory.swap.bytes.available` | `sysdig_host_memory_swap_available_bytes `                   |
| `memory.swap.bytes.total`     | `sysdig_host_memory_swap_total_bytes`                        |
| `memory.swap.bytes.used`      | `sysdig_host_memory_swap_used_bytes`                         |
| `memory.swap.used.percent`    | `sysdig_host_memory_swap_used_percent`                       |
| `memory.used.percent`         | `sysdig_host_memory_used_percent`<br/>`sysdig_container_memory_used_percent`<br/>`sysdig_program_memory_used_percent` |
| `file.bytes.in`               | `sysdig_host_file_in_bytes`<br/>`sysdig_container_file_in_bytes`</br> `sysdig_program_file_in_bytes` |
| `file.bytes.out`              | `sysdig_host_file_out_bytes`<br/>`sysdig_container_file_out_bytes`</br> `sysdig_program_file_out_bytes` |
| `file.bytes.total`            | `sysdig_host_file_bytes_total`<br/>`sysdig_container_file_bytes_total`</br> `sysdig_program_file_bytes_total` |
| `file.iops.in`                | `sysdig_host_file_in_iops`<br/>`sysdig_container_file_in_iops`</br>  |
| `file.iops.out`               | `sysdig_host_file_out_iops`<br/>`sysdig_container_file_out_iops`</br> |
| `file.iops.total`             | `sysdig_host_file_iops_total`<br/>`sysdig_container_file_iops_total`</br> `sysdig_program_file_iops_total` |
| `file.open.count`             | `sysdig_host_file_open_count`<br/>`sysdig_container_file_open_count` |
| `file.time.in`                | `sysdig_host_file_in_time`<br/>`sysdig_container_file_in_time` |
| `file.time.out`               | `sysdig_host_file_out_time`<br/>`sysdig_container_file_out_time` |
| `file.time.total`             | `sysdig_host_file_time_total`<br/>`sysdig_container_file_time_total`</br> `sysdig_program_file_time_total` |
| `fs.bytes.free`               | `sysdig_host_fs_free_bytes`<br/>`sysdig_container_fs_free_bytes`<br/>`sysdig_fs_free_bytes` |
| `fs.bytes.total`              | `sysdig_fs_total_bytes`<br>`sysdig_host_fs_total_bytes`<br>`sysdig_container_fs_total_bytes` |
| `fs.bytes.used`               | `sysdig_fs_used_bytes`<br>`sysdig_host_fs_used_bytes`<br/>`sysdig_container_fs_used_bytes` |
| `fs.free.percent`             | `sysdig_fs_free_percent`<br>`sysdig_host_fs_free_percent`<br/>`sysdig_container_fs_free_percent` |
| `fs.inodes.total.count`       | `sysdig_fs_inodes_total_count`</br> `sysdig_container_fs_inodes_total_count`<br>`sysdig_host_fs_inodes_total_count` |
| `fs.inodes.used.count`        | `sysdig_fs_inodes_used_count`</br> `sysdig_container_fs_inodes_used_count`<br/>`sysdig_host_fs_inodes_used_count` |
| `fs.inodes.used.percent`      | `sysdig_fs_inodes_used_percent`</br> `sysdig_container_fs_inodes_used_percent`<br/>`sysdig_host_fs_inodes_used_percent` |
| `fs.largest.used.percent`     | `sysdig_container_fs_largest_used_percent`</br> `sysdig_host_fs_largest_used_percent` |
| `fs.root.used.percent`        | `sysdig_container_fs_root_used_percent`</br> `sysdig_host_fs_root_used_percent` |
| `fs.used.percent`             | `sysdig_fs_used_percent`</br> `sysdig_container_fs_used_percent`</br> `sysdig_host_fs_used_percent` |
| `net.bytes.in`                | `sysdig_host_net_in_bytes`<br/>`sysdig_container_net_in_bytes`<br/>`sysdig_program_net_in_bytes` |
| `net.bytes.out`               | `sysdig_host_net_out_bytes`<br/>`sysdig_container_net_out_bytes`<br/>`sysdig_program_net_out_bytes` |
| `net.bytes.total`             | `sysdig_host_net_total_bytes`<br/>`sysdig_container_net_total_bytes`<br/>`sysdig_program_net_total_bytes`<br/>`sysdig_connection_net_total_bytes` |
| `proc.count`                  | `sysdig_host_proc_count`<br/>`sysdig_container_proc_count`<br/>`sysdig_program_proc_count` |
| `thread.count`                | `sysdig_host_thread_count`<br/>`sysdig_container_thread_count`<br/>`sysdig_program_thread_count` |
| `container.count`             | `sysdig_container_count`                                     |
| `system.uptime`               | `sysdig_host_system_uptime`                                  |
| `uptime`                      | `sysdig_host_up`<br/>`sysdig_container_up`<br/>`sysdig_program_up` |
