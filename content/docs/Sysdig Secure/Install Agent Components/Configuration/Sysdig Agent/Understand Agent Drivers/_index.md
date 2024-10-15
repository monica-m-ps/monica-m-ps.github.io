---
linkTitle: "Understand Agent Drivers"
title: "Understand Agent Drivers"
weight: 10
aliases:
 - /en/understand-agent-driver-options.html
 - /en/understand-agent-drivers
 - /en/docs/installation/sysdig-agent/agent-configuration/understand-agent-drivers/
 - /en/docs/installation/configuration/sysdig-agent/understand-agent-drivers/
---

The Sysdig Agent can receive system call events from the Linux kernel via one of three different drivers: 

- Kernel module (kmod)
- eBPF
- Universal eBPF

Each driver has its own prerequisites, advantages, and trade-offs.

All three drivers share common features. They attach event handlers to functions within the Linux kernel, such as a system call entry point or exit point. They also allocate buffers from the instrumented system's RAM to send kernel events to the agent program running in the user space. However, they differ in the way they implement these common features.

### Kernel Module
The kernel module driver is what its name suggests: an additional driver that is inserted into the Linux kernel. It implements kernel event handlers and a custom channel for sending event data to the agent's user space program. It must be built against the headers for the specific kernel it will run on. The kernel module also allocates its event buffers from kernel memory, giving it the lowest user-visible memory footprint.

### eBPF
The current eBPF driver uses the same runtime and kernel instrumentation techniques as Universal eBPF.  However, it relies on older kernel features which limits its efficiency and capabilities. Like the kernel module driver, the current eBPF driver must be built against headers for the specific kernel it will run on. It differs, however, in that the current eBPF driver must allocate event buffers from memory in userspace. This increases the current eBPF driver's user-visible memory footprint by 8MiB per CPU, relative to the kernel module.

### Universal eBPF
The Universal eBPF driver is a suite of programs that run in a special virtual machine within the Linux kernel and attaches to the same kernel functions as the kmod event handlers do. eBPF programs exchange data with user space and each other through a collection of eBPF specific data structures including maps and ring buffers. The Universal eBPF driver is pre-built and embedded in the agent binary. It does not require kernel headers from the target system to function. 

However, it does rely on kernel features that are only available in upstream kernel versions 5.8 and newer.  The kernel must also be built with embedded BPF Type Format, (BTF), information at `/sys/kernel/btf/vmlinux`.  Unlike the kernel module, the Universal eBPF driver must allocate event buffers from memory in userspace.  This increases the Universal eBPF driver's user-visible memory footprint by 8 [MiB](https://physics.nist.gov/cuu/Units/binary.html) per 2 CPU cores, relative to the 8MiB per 1 CPU core for kernel modules.

### Driver Comparison Table

<table>
    <tr>
        <td><b>Driver Name</b></td>
        <td><b>Minimum Compatible Kernel Version</b></td>
        <td><b>Requires Kernel Headers</b></td>
        <td><b>Event Buffer User Space Memory Footprint</b></td>
        <td><b>Ideal Use Case</b></td>
    </tr>
    <tr>
        <td>Kernel Module</td>
        <td>2.6</td>
        <td>Yes</td>
        <td>N/A</td>
        <td>Systems with pre-5.8 Linux kernels</td>
    </tr>
    <tr>
        <td>eBPF</td>
        <td>4.14</td>
        <td>Yes</td>
        <td>8MiB per CPU core</td>
        <td>x86 systems with Linux kernel version 4.14 to 5.7 where third-party kernel modules are not allowed.</td>
    </tr>
    <tr>
        <td>Universal eBPF</td>
        <td>5.8</td>
        <td>No</td>
        <td>8MiB per 2 CPU cores</td>
        <td>New agent installations on systems with Linux kernel version 5.8 or newer.</td>
    </tr>
</table>

{{% callout type="note" %}}

Agent driver compatibility is described in terms of upstream or "vanilla" kernel versions because some popular Linux distributions back-port features from newer kernels into the older kernel versions they ship with their distributions. RHEL is especially well-known for this practice.

{{% /callout %}}

### Memory Accounting in the Linux Kernel
Examining the memory statistics can give a false impression that the agent using eBPF drivers consumes more memory than an agent using the kernel module due to how the Linux kernel accounts for memory. The Linux kernel allocates memory for the agent kernel module under `vmalloc`,  found in the `VmallocUsed` header in the `/proc/meminfo` file. In contrast, the agent's userspace memory usage directly includes the memory used by the eBPF probes. When switching from the kmod driver to eBPF drivers, you need to increase orchestrator memory limits according to the [Driver Comparison Table](#driver-comparison-table). Despite this increase, the total system memory usage remains lowest with the universal eBPF drivers.

For example, let us compare the memory use between kmod and universal eBPF drivers on a 64-core machine:
```
# Agent with kmod driver
cat /proc/meminfo | grep "VmallocUsed"
VmallocUsed:      801432 kB

# Orchestrator reported memory usage, in MiB
kubectl top pods --containers -n [sysdig agent namespace] | awk '{print $4}'
327
```

```
# Agent with universal eBPF driver
cat /proc/meminfo | grep "VmallocUsed"
VmallocUsed:      287692 kB

# Orchestrator reported memory usage, in MiB
kubectl top pods --containers -n [sysdig agent namespace] | awk '{print $4}'
596
```

Kernel memory is reduced by 801432 - 287692 = 513740 kB, and userspace memory increased by 596 - 327 = 269 MiB, for a net reduction of 233 MiB in overall memory use. The amount of memory reduction is linearly dependent on the number of cores.

For information on configuration, see [Configuration Library](/en/configuration-library.html).

### Learn More
* [What is eBPF? An Introduction and Deep Dive into eBPF the Technology](https://ebpf.io/what-is-ebpf/)
* [What is the Difference Between User Space and Kernel Space in Linux](https://itslinuxfoss.com/difference-between-user-space-kernel-space-linux/)
* [Prefixes for binary multiples](https://physics.nist.gov/cuu/Units/binary.html)
* [Configuration Library](/en/configuration-library.html)

