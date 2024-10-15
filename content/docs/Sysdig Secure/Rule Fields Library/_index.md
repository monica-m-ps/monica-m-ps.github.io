---
linkTitle: "Fields Library for Threat Detection Rules"
title: "Fields Library for Threat Detection Rules"
weight: "13"
No_list: "true"
aliases:
  - /en/secure-reference-library
description: "Sysdig Secure enables you to create and customize Threat Detection Rules to secure your environment. A rule file, formatted in YAML, comprises conditions and outputs. You can specify event fields within the rule to define conditions and outputs. This topic covers the fields supported by Sysdig Secure, including those defined in the output key of a rule, which are also displayed in the associated events in the Event feed."
---

In a Threat Detection rule, a field refers to a specific attribute of an event captured from the system call. You use fields to define conditions and outputs within the rules, allowing you to specify what events should trigger events or actions based on the specified conditions. These fields help Sysdig Secure identify and respond to suspicious or unauthorized activities within your environment. This topic provides a comprehensive understanding of all the supported fields that you can include in a Threat Detection rule.

## Field Class: evt

These fields can be used for all event types.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`evt.num` | UINT64 | The event number.
`evt.time` | CHARBUF | The event timestamp as a time string that includes the nanosecond part.
`evt.time.s` | CHARBUF | The event timestamp as a time string with no nanoseconds.
`evt.time.iso8601` | CHARBUF | The event timestamp in ISO 8601 format, including nanoseconds and time zone offset (in UTC).
`evt.datetime` | CHARBUF | The event timestamp as a time string that includes the date.
`evt.datetime.s` | CHARBUF | The event timestamp as a datetime string with no nanoseconds.
`evt.rawtime` | ABSTIME | The absolute event timestamp, i.e. nanoseconds from epoch.
`evt.rawtime.s` | ABSTIME | The integer part of the event timestamp (e.g. seconds since epoch).
`evt.rawtime.ns` | ABSTIME | The fractional part of the absolute event timestamp.
`evt.reltime` | RELTIME | The number of nanoseconds from the beginning of the capture.
`evt.reltime.s` | RELTIME | The number of seconds from the beginning of the capture.
`evt.reltime.ns` | RELTIME | The fractional part (in ns) of the time from the beginning of the capture.
`evt.pluginname` | CHARBUF | if the event comes from a plugin-defined event source, the name of the plugin that generated it. The plugin must be currently loaded.
`evt.plugininfo` | CHARBUF | if the event comes from a plugin-defined event source, a summary of the event as formatted by the plugin. The plugin must be currently loaded.
`evt.source` | CHARBUF | The name of the source that produced the event.
`evt.is_async` | BOOL | 'true' for asynchronous events, 'false' otherwise.
`evt.asynctype` | CHARBUF | If the event is asynchronous, the type of the event (e.g. 'container').
`evt.hostname` | CHARBUF | The hostname of the underlying host can be customized by setting an environment variable (e.g. FALCO_HOSTNAME for the Falco agent). This is valuable in Kubernetes setups, where the hostname can match the pod name particularly in DaemonSet deployments. To achieve this, assign Kubernetes' spec.nodeName to the environment variable. Notably, spec.nodeName generally includes the cluster name.

## Field Class: evt

The event fields that are applicable to syscall events. Note that for most events you can access the individual arguments of each syscall via `evt.arg`. For example, `evt.arg.filename`.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`evt.latency` | RELTIME | The delta between an exit event and the correspondent enter event, in nanoseconds.
`evt.latency.s` | RELTIME | The integer part of the event latency delta.
`evt.latency.ns` | RELTIME | The fractional part of the event latency delta.
`evt.latency.human` | CHARBUF | The delta between an exit event and the correspondent enter event, as a human readable string (e.g. 10.3ms).
`evt.deltatime` | RELTIME | The delta between this event and the previous event, in nanoseconds.
`evt.deltatime.s` | RELTIME | The integer part of the delta between this event and the previous event.
`evt.deltatime.ns` | RELTIME | The fractional part of the delta between this event and the previous event.
`evt.dir` | CHARBUF | The event direction can be either '>' for enter events or '<' for exit events.
`evt.type` | CHARBUF | The name of the event. For example, 'open'.
`evt.type.is` | UINT32 | Allows yoy to specify an event type, and returns `1` for events that are of that type. For example, `evt.type.is.open` returns `1` for open events, `0` for any other event.
`syscall.type` | CHARBUF | For system call events, the name of the system call. For example, 'open'. Unset for other events. For example, switch or internal events. Use this field instead of `evt.type` if you need to make sure that the filtered/printed value is actually a system call.
`evt.category` | CHARBUF | The event category. Example values are 'file' (for file operations like open and close), 'net' (for network operations like socket and bind), memory (for things like brk or mmap), and so on.
`evt.cpu` | INT16 | The number of the CPU where this event happened.
`evt.args` | CHARBUF | All the event arguments, aggregated into a single string.
`evt.arg` | CHARBUF | One of the event arguments specified by name or by number. Some events, such as return codes or FDs will be converted into a text representation when possible. E.g. 'evt.arg.fd' or 'evt.arg[0]'.
`evt.rawarg` | DYNAMIC | One of the event arguments specified by name. For example, 'evt.rawarg.fd'.
`evt.info` | CHARBUF | For most events, this field returns the same value as `evt.args`. However, for some events, such as those write to `/dev/log`, it provides higher level of information coming from decoding the arguments.
`evt.buffer` | BYTEBUF | The binary data buffer for events that have one, like read(), recvfrom(), etc. Use this field in filters with 'contains' to search into I/O data buffers.
`evt.buflen` | UINT64 | The length of the binary data buffer for events that have one, like read(), recvfrom(), etc.
`evt.res` | CHARBUF | event return value, as a string. If the event failed, the result is an error code string, such as 'ENOENT', otherwise the result is the string 'SUCCESS'.
`evt.rawres` | INT64 | The event returns value as a number. For example, -2. Useful for range comparisons.
`evt.failed` | BOOL | 'true' for events that returned an error status.
`evt.is_io` | BOOL | 'true' for events that read or write to FDs, like read(), send, recvfrom(), etc.
`evt.is_io_read` | BOOL | 'true' for events that read from FDs, like read(), recv(), recvfrom(), etc.
`evt.is_io_write` | BOOL | 'true' for events that write to FDs, like write(), send(), etc.
`evt.io_dir` | CHARBUF | 'r' for events that read from FDs, like read(); 'w' for events that write to FDs, like write().
`evt.is_wait` | BOOL | 'true' for events that make the thread wait, e.g. sleep(), select(), poll().
`evt.wait_latency` | RELTIME | For events that make the thread wait (e.g. sleep(), select(), poll()), this is the time spent waiting for the event to return, in nanoseconds.
`evt.is_syslog` | BOOL | 'true' for events that are writes to /dev/log.
`evt.count` | UINT32 | This filter field always returns 1.
`evt.count.error` | UINT32 | This filter field returns 1 for events that returned with an error.
`evt.count.error.file` | UINT32 | This filter field returns 1 for events that returned with an error and are related to file I/O.
`evt.count.error.net` | UINT32 | This filter field returns 1 for events that returned with an error and are related to network I/O.
`evt.count.error.memory` | UINT32 | This filter field returns 1 for events that returned with an error and are related to memory allocation.
`evt.count.error.other` | UINT32 | This filter field returns 1 for events that returned with an error and are related to none of the previous categories.
`evt.count.exit` | UINT32 | This filter field returns 1 for exit events.
`evt.around` | UINT64 | Accepts the event if it's around the specified time interval. The syntax is evt.around[T]=D, where T is the value returned by %evt.rawtime for the event and D is a delta in milliseconds. For example, evt.around[1404996934793590564]=1000 will return the events with timestamp with one second before the timestamp and one second after it, for a total of two seconds of capture.
`evt.abspath` | CHARBUF | Absolute path calculated from dirfd and name during syscalls like renameat and symlinkat. Use 'evt.abspath.src' or 'evt.abspath.dst' for syscalls that support multiple paths.
`evt.is_open_read` | BOOL | 'true' for open/openat/openat2/open_by_handle_at events where the path was opened for reading
`evt.is_open_write` | BOOL | 'true' for open/openat/openat2/open_by_handle_at events where the path was opened for writing
`evt.is_open_exec` | BOOL | 'true' for open/openat/openat2/open_by_handle_at or creat events where a file is created with execute permissions
`evt.is_open_create` | BOOL | 'true' for for open/openat/openat2/open_by_handle_at events where a file is created.

## Field Class: process

Additional information about the process and thread executing the syscall event.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`proc.exe` | CHARBUF | The first command-line argument (i.e., argv[0]), typically the executable name or a custom string as specified by the user. It is primarily obtained from syscall arguments, truncated after 4096 bytes, or, as a fallback, by reading /proc/PID/cmdline, in which case it may be truncated after 1024 bytes. This field may differ from the last component of proc.exepath, reflecting how command invocation and execution paths can vary.
`proc.pexe` | CHARBUF | The proc.exe (first command line argument argv[0]) of the parent process.
`proc.aexe` | CHARBUF | The proc.exe (first command line argument argv[0]) for a specific process ancestor. You can access different levels of ancestors by using indices. For example, proc.aexe[1] retrieves the proc.exe of the parent process, proc.aexe[2] retrieves the proc.exe of the grandparent process, and so on. The current process's proc.exe line can be obtained using proc.aexe[0]. When used without any arguments, proc.aexe is applicable only in filters and matches any of the process ancestors. For instance, you can use `proc.aexe endswith java` to match any process ancestor whose proc.exe ends with the term `java`.
`proc.exepath` | CHARBUF | The full executable path of a process, resolving to the canonical path for symlinks. This is primarily obtained from the kernel, or as a fallback, by reading /proc/PID/exe (in the latter case, the path is truncated after 1024 bytes). For eBPF drivers, due to verifier limits, path components may be truncated to 24 for legacy eBPF on kernel <5.2, 48 for legacy eBPF on kernel >=5.2, or 96 for modern eBPF.
`proc.pexepath` | CHARBUF | The proc.exepath (full executable path) of the parent process.
`proc.aexepath` | CHARBUF | The proc.exepath (full executable path) for a specific process ancestor. You can access different levels of ancestors by using indices. For example, proc.aexepath[1] retrieves the proc.exepath of the parent process, proc.aexepath[2] retrieves the proc.exepath of the grandparent process, and so on. The current process's proc.exepath line can be obtained using proc.aexepath[0]. When used without any arguments, proc.aexepath is applicable only in filters and matches any of the process ancestors. For instance, you can use `proc.aexepath endswith java` to match any process ancestor whose path ends with the term `java`.
`proc.name` | CHARBUF | The process name (truncated after 16 characters) generating the event (task->comm). Truncation is determined by kernel settings and not by Falco. This field is collected from the syscalls args or, as a fallback, extracted from /proc/PID/status. The name of the process and the name of the executable file on disk (if applicable) can be different if a process is given a custom name which is often the case for example for java applications.
`proc.pname` | CHARBUF | The proc.name truncated after 16 characters) of the process generating the event.
`proc.aname` | CHARBUF | The proc.name (truncated after 16 characters) for a specific process ancestor. You can access different levels of ancestors by using indices. For example, proc.aname[1] retrieves the proc.name of the parent process, proc.aname[2] retrieves the proc.name of the grandparent process, and so on. The current process's proc.name line can be obtained using proc.aname[0]. When used without any arguments, proc.aname is applicable only in filters and matches any of the process ancestors. For instance, you can use `proc.aname=bash` to match any process ancestor whose name is `bash`.
`proc.args` | CHARBUF | The arguments passed on the command line when starting the process generating the event excluding argv[0] (truncated after 4096 bytes). This field is collected from the syscalls args or, as a fallback, extracted from /proc/PID/cmdline.
`proc.cmdline` | CHARBUF | The concatenation of `proc.name + proc.args` (truncated after 4096 bytes) when starting the process generating the event.
`proc.pcmdline` | CHARBUF | The proc.cmdline (full command line (proc.name + proc.args)) of the parent of the process generating the event.
`proc.acmdline` | CHARBUF | The full command line (proc.name + proc.args) for a specific process ancestor. You can access different levels of ancestors by using indices. For example, proc.acmdline[1] retrieves the full command line of the parent process, proc.acmdline[2] retrieves the proc.cmdline of the grandparent process, and so on. The current process's full command line can be obtained using proc.acmdline[0]. When used without any arguments, proc.acmdline is applicable only in filters and matches any of the process ancestors. For instance, you can use `proc.acmdline contains base64` to match any process ancestor whose command line contains the term base64.
`proc.cmdnargs` | UINT64 | The number of command line args (proc.args).
`proc.cmdlenargs` | UINT64 | The total count of characters / length of the command line args (proc.args) combined excluding whitespaces between args.
`proc.exeline` | CHARBUF | The full command line, with exe as first argument (proc.exe + proc.args) when starting the process generating the event.
`proc.env` | CHARBUF | The environment variables of the process generating the event as concatenated string 'ENV_NAME=value ENV_NAME1=value1'. Can also be used to extract the value of a known env variable, e.g. proc.env[ENV_NAME].
`proc.aenv` | CHARBUF | [EXPERIMENTAL] This field can be used in three flavors: (1) as a filter checking all parents, e.g. 'proc.aenv contains xyz', which is similar to the familiar 'proc.aname contains xyz' approach, (2) checking the `proc.env` of a specified level of the parent, e.g. 'proc.aenv[2]', which is similar to the familiar 'proc.aname[2]' approach, or (3) checking the first matched value of a known ENV_NAME in the parent lineage, such as 'proc.aenv[ENV_NAME]' (across a max of 20 ancestor levels). This field may be deprecated or undergo breaking changes in future releases. Please use it with caution.
`proc.cwd` | CHARBUF | The current working directory of the event.
`proc.loginshellid` | INT64 | The pid of the oldest shell among the ancestors of the current process, if there is one. This field can be used to separate different user sessions.
`proc.tty` | UINT32 | The controlling terminal of the process. 0 for processes without a terminal.
`proc.pid` | INT64 | The id of the process generating the event.
`proc.ppid` | INT64 | The pid of the parent of the process generating the event.
`proc.apid` | INT64 | The pid for a specific process ancestor. You can access different levels of ancestors by using indices. For example, proc.apid[1] retrieves the pid of the parent process, proc.apid[2] retrieves the pid of the grandparent process, and so on. The current process's pid can be obtained using proc.apid[0]. When used without any arguments, proc.apid is applicable only in filters and matches any of the process ancestors. For instance, you can use `proc.apid=1337` to match any process ancestor whose pid is equal to 1337.
`proc.vpid` | INT64 | The id of the process generating the event as seen from its current PID namespace.
`proc.pvpid` | INT64 | The id of the parent process generating the event as seen from its current PID namespace.
`proc.sid` | INT64 | The session id of the process generating the event.
`proc.sname` | CHARBUF | The name of the current process's session leader. This is either the process with pid=proc.sid or the eldest ancestor that has the same sid as the current process.
`proc.sid.exe` | CHARBUF | The first command line argument argv[0] (usually the executable name or a custom one) of the current process's session leader. This is either the process with pid=proc.sid or the eldest ancestor that has the same sid as the current process.
`proc.sid.exepath` | CHARBUF | The full executable path of the current process's session leader. This is either the process with pid=proc.sid or the eldest ancestor that has the same sid as the current process.
`proc.vpgid` | INT64 | The process group id of the process generating the event, as seen from its current PID namespace.
`proc.vpgid.name` | CHARBUF | The name of the current process's process group leader. This is either the process with proc.vpgid == proc.vpid or the eldest ancestor that has the same vpgid as the current process. The description of `proc.is_vpgid_leader` offers additional insights.
`proc.vpgid.exe` | CHARBUF | The first command line argument argv[0] (usually the executable name or a custom one) of the current process's process group leader. This is either the process with proc.vpgid == proc.vpid or the eldest ancestor that has the same vpgid as the current process. The description of `proc.is_vpgid_leader` offers additional insights.
`proc.vpgid.exepath` | CHARBUF | The full executable path of the current process's process group leader. This is either the process with proc.vpgid == proc.vpid or the eldest ancestor that has the same vpgid as the current process. The description of `proc.is_vpgid_leader` offers additional insights.
`proc.duration` | RELTIME | Number of nanoseconds since the process started.
`proc.ppid.duration` | RELTIME | Number of nanoseconds since the parent process started.
`proc.pid.ts` | RELTIME | Start of process as epoch timestamp in nanoseconds.
`proc.ppid.ts` | RELTIME | Start of parent process as epoch timestamp in nanoseconds.
`proc.is_exe_writable` | BOOL | 'true' if this process' executable file is writable by the same user that spawned the process.
`proc.is_exe_upper_layer` | BOOL | 'true' if this process' executable file is in upper layer in overlayfs. This field value can only be trusted if the underlying kernel version is greater or equal than 3.18.0, since overlayfs was introduced at that time.
`proc.is_exe_from_memfd` | BOOL | 'true' if the executable file of the current process is an anonymous file created using memfd_create() and is being executed by referencing its file descriptor (fd). This type of file exists only in memory and not on disk. Relevant to detect malicious in-memory code injection. Requires kernel version greater or equal to 3.17.0.
`proc.is_sid_leader` | BOOL | 'true' if this process is the leader of the process session, proc.sid == proc.vpid. For host processes vpid reflects pid.
`proc.is_vpgid_leader` | BOOL | 'true' if this process is the leader of the virtual process group, proc.vpgid == proc.vpid. For host processes vpgid and vpid reflect pgid and pid. Can help to distinguish if the process was 'directly' executed for instance in a tty (similar to bash history logging, `is_vpgid_leader` would be 'true') or executed as descendent process in the same process group which for example is the case when subprocesses are spawned from a script (`is_vpgid_leader` would be 'false').
`proc.exe_ino` | INT64 | The inode number of the executable file on disk. Can be correlated with fd.ino.
`proc.exe_ino.ctime` | ABSTIME | Last status change time of executable file (inode->ctime) as epoch timestamp in nanoseconds. Time is changed by writing or by setting inode information e.g. owner, group, link count, mode etc.
`proc.exe_ino.mtime` | ABSTIME | Last modification time of executable file (inode->mtime) as epoch timestamp in nanoseconds. Time is changed by file modifications, e.g. by mknod, truncate, utime, write of more than zero bytes etc. For tracking changes in owner, group, link count or mode, use proc.exe_ino.ctime instead.
`proc.exe_ino.ctime_duration_proc_start` | ABSTIME | Number of nanoseconds between modifying status of executable image and spawning a new process using the changed executable image.
`proc.exe_ino.ctime_duration_pidns_start` | ABSTIME | Number of nanoseconds between PID namespace start ts and ctime exe file if PID namespace start predates ctime.
`proc.pidns_init_start_ts` | UINT64 | Start of PID namespace (container or non container pid namespace) as epoch timestamp in nanoseconds.
`thread.cap_permitted` | CHARBUF | The permitted capabilities set
`thread.cap_inheritable` | CHARBUF | The inheritable capabilities set
`thread.cap_effective` | CHARBUF | The effective capabilities set
`proc.is_container_healthcheck` | BOOL | 'true' if this process is running as a part of the container's health check.
`proc.is_container_liveness_probe` | BOOL | 'true' if this process is running as a part of the container's liveness probe.
`proc.is_container_readiness_probe` | BOOL | 'true' if this process is running as a part of the container's readiness probe.
`proc.fdopencount` | UINT64 | Number of open FDs for the process
`proc.fdlimit` | INT64 | Maximum number of FDs the process can open.
`proc.fdusage` | DOUBLE | The ratio between open FDs and maximum available FDs for the process.
`proc.vmsize` | UINT64 | Total virtual memory for the process (as kb).
`proc.vmrss` | UINT64 | Resident non-swapped memory for the process (as kb).
`proc.vmswap` | UINT64 | Swapped memory for the process (as kb).
`thread.pfmajor` | UINT64 | Number of major page faults since thread start.
`thread.pfminor` | UINT64 | Number of minor page faults since thread start.
`thread.tid` | INT64 | The id of the thread generating the event.
`thread.ismain` | BOOL | 'true' if the thread generating the event is the main one in the process.
`thread.vtid` | INT64 | The id of the thread generating the event as seen from its current PID namespace.
`thread.exectime` | RELTIME | CPU time spent by the last scheduled thread, in nanoseconds. Exported by switch events only.
`thread.totexectime` | RELTIME | Total CPU time, in nanoseconds since the beginning of the capture, for the current thread. Exported by switch events only.
`thread.cgroups` | CHARBUF | All cgroups the thread belongs to, aggregated into a single string.
`thread.cgroup` | CHARBUF | The cgroup the thread belongs to, for a specific subsystem. e.g. thread.cgroup.cpuacct.
`proc.nthreads` | UINT64 | The number of alive threads that the process generating the event currently has, including the leader thread. Please note that the leader thread may not be here, in that case 'proc.nthreads' and 'proc.nchilds' are equal
`proc.nchilds` | UINT64 | The number of alive not leader threads that the process generating the event currently has. This excludes the leader thread.
`thread.cpu` | DOUBLE | The CPU consumed by the thread in the last second.
`thread.cpu.user` | DOUBLE | The user CPU consumed by the thread in the last second.
`thread.cpu.system` | DOUBLE | The system CPU consumed by the thread in the last second.
`thread.vmsize` | UINT64 | For the process main thread, this is the total virtual memory for the process (as kb). For the other threads, this field is zero.
`thread.vmrss` | UINT64 | For the process main thread, this is the resident non-swapped memory for the process (as kb). For the other threads, this field is zero.

## Field Class: user

Information about the user executing the specific event.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`user.uid` | UINT32 | user ID.
`user.name` | CHARBUF | user name.
`user.homedir` | CHARBUF | home directory of the user.
`user.shell` | CHARBUF | user's shell.
`user.loginuid` | INT64 | audit user id (auid), internally the loginuid is of type `uint32_t`. However, if an invalid uid corresponding to UINT32_MAX is encountered, it is returned as -1 to support familiar filtering conditions.
`user.loginname` | CHARBUF | audit user name (auid).

## Field Class: group

Information about the user group.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`group.gid` | UINT32 | group ID.
`group.name` | CHARBUF | group name.

## Field Class: container

Container information. If the event is not happening inside a container, both id and name will be set to 'host'.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`container.id` | CHARBUF | The truncated container ID (first 12 characters), e.g. 3ad7b26ded6d is extracted from the Linux cgroups by Falco within the kernel. Consequently, this field is reliably available and serves as the lookup key for Falco's synchronous or asynchronous requests against the container runtime socket to retrieve all other 'container.*' information. One important aspect to be aware of is that if the process occurs on the host, meaning not in the container PID namespace, this field is set to a string called 'host'. In Kubernetes, pod sandbox container processes can exist where `container.id` matches `k8s.pod.sandbox_id`, lacking other 'container.*' details.
`container.full_id` | CHARBUF | The full container ID, e.g. 3ad7b26ded6d8e7b23da7d48fe889434573036c27ae5a74837233de441c3601e. In contrast to `container.id`, we enrich this field as part of the container engine enrichment. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.name` | CHARBUF | The container name. In instances of userspace container engine lookup delays, this field may not be available yet. One important aspect to be aware of is that if the process occurs on the host, meaning not in the container PID namespace, this field is set to a string called 'host'.
`container.image` | CHARBUF | The container image name (e.g. falcosecurity/falco:latest for docker). In instances of userspace container engine lookup delays, this field may not be available yet.
`container.image.id` | CHARBUF | The container image id (e.g. 6f7e2741b66b). In instances of userspace container engine lookup delays, this field may not be available yet.
`container.type` | CHARBUF | The container type, e.g. docker, cri-o, containerd etc.
`container.privileged` | BOOL | 'true' for containers running as privileged, 'false' otherwise. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mounts` | CHARBUF | A space-separated list of mount information. Each item in the list has the format 'source:dest:mode:rdrw:propagation'. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mount` | CHARBUF | Information about a single mount, specified by number (e.g. container.mount[0]) or mount source (container.mount[/usr/local]). The pathname can be a glob (container.mount[/usr/local/*]), in which case the first matching mount will be returned. The information has the format 'source:dest:mode:rdrw:propagation'. If there is no mount with the specified index or matching the provided source, returns the string "none" instead of a NULL value. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mount.source` | CHARBUF | The mount source, specified by number (e.g. container.mount.source[0]) or mount destination (container.mount.source[/host/lib/modules]). The pathname can be a glob. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mount.dest` | CHARBUF | The mount destination, specified by number (e.g. container.mount.dest[0]) or mount source (container.mount.dest[/lib/modules]). The pathname can be a glob. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mount.mode` | CHARBUF | The mount mode, specified by number (e.g. container.mount.mode[0]) or mount source (container.mount.mode[/usr/local]). The pathname can be a glob. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mount.rdwr` | CHARBUF | The mount rdwr value, specified by number (e.g. container.mount.rdwr[0]) or mount source (container.mount.rdwr[/usr/local]). The pathname can be a glob. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.mount.propagation` | CHARBUF | The mount propagation value, specified by number (e.g. container.mount.propagation[0]) or mount source (container.mount.propagation[/usr/local]). The pathname can be a glob. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.image.repository` | CHARBUF | The container image repository (e.g. falcosecurity/falco). In instances of userspace container engine lookup delays, this field may not be available yet.
`container.image.tag` | CHARBUF | The container image tag (e.g. stable, latest). In instances of userspace container engine lookup delays, this field may not be available yet.
`container.image.digest` | CHARBUF | The container image registry digest (e.g. sha256:d977378f890d445c15e51795296e4e5062f109ce6da83e0a355fc4ad8699d27). In instances of userspace container engine lookup delays, this field may not be available yet.
`container.healthcheck` | CHARBUF | The container's health check. Will be the null value ("N/A") if no healthcheck configured, "NONE" if configured but explicitly not created, and the healthcheck command line otherwise. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.liveness_probe` | CHARBUF | The container's liveness probe. Will be the null value ("N/A") if no liveness probe configured, the liveness probe command line otherwise. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.readiness_probe` | CHARBUF | The container's readiness probe. Will be the null value ("N/A") if no readiness probe configured, the readiness probe command line otherwise. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.start_ts` | UINT64 | Container start as epoch timestamp in nanoseconds based on proc.pidns_init_start_ts and extracted in the kernel and not from the container runtime socket / container engine.
`container.duration` | RELTIME | Number of nanoseconds since container.start_ts.
`container.ip` | CHARBUF | The container's / pod's primary ip address as retrieved from the container engine. Only ipv4 addresses are tracked. Consider container.cni.json (CRI use case) for logging ip addresses for each network interface. In instances of userspace container engine lookup delays, this field may not be available yet.
`container.cni.json` | CHARBUF | The container's / pod's CNI result field from the respective pod status info. It contains ip addresses for each network interface exposed as unparsed escaped JSON string. Supported for CRI container engine (containerd, cri-o runtimes), optimized for containerd (some non-critical JSON keys removed). Useful for tracking ips (ipv4 and ipv6, dual-stack support) for each network interface (multi-interface support). In instances of userspace container engine lookup delays, this field may not be available yet.

## Field Class: fd

Every syscall that has a file descriptor in its arguments has these fields set with information related to the file.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`fd.num` | INT64 | the unique number identifying the file descriptor.
`fd.type` | CHARBUF | type of FD. Can be 'file', 'directory', 'ipv4', 'ipv6', 'unix', 'pipe', 'event', 'signalfd', 'eventpoll', 'inotify'  'signalfd' or 'memfd'.
`fd.typechar` | CHARBUF | type of FD as a single character. Can be 'f' for file, 4 for IPv4 socket, 6 for IPv6 socket, 'u' for unix socket, p for pipe, 'e' for eventfd, 's' for signalfd, 'l' for eventpoll, 'i' for inotify, 'b' for bpf, 'u' for userfaultd, 'r' for io_uring, 'm' for memfd ,'o' for unknown.
`fd.name` | CHARBUF | FD full name. If the fd is a file, this field contains the full path. If the FD is a socket, this field contain the connection tuple.
`fd.directory` | CHARBUF | If the fd is a file, the directory that contains it.
`fd.filename` | CHARBUF | If the fd is a file, the filename without the path.
`fd.ip` | IPADDR | matches the ip address (client or server) of the fd.
`fd.cip` | IPADDR | client IP address.
`fd.sip` | IPADDR | server IP address.
`fd.lip` | IPADDR | local IP address.
`fd.rip` | IPADDR | remote IP address.
`fd.port` | PORT | matches the port (either client or server) of the fd.
`fd.cport` | PORT | for TCP/UDP FDs, the client port.
`fd.sport` | PORT | for TCP/UDP FDs, server port.
`fd.lport` | PORT | for TCP/UDP FDs, the local port.
`fd.rport` | PORT | for TCP/UDP FDs, the remote port.
`fd.l4proto` | CHARBUF | the IP protocol of a socket. Can be 'tcp', 'udp', 'icmp' or 'raw'.
`fd.sockfamily` | CHARBUF | the socket family for socket events. Can be 'ip' or 'unix'.
`fd.is_server` | BOOL | 'true' if the process owning this FD is the server endpoint in the connection.
`fd.uid` | CHARBUF | a unique identifier for the FD, created by chaining the FD number and the thread ID.
`fd.containername` | CHARBUF | chaining of the container ID and the FD name. Useful when trying to identify which container an FD belongs to.
`fd.containerdirectory` | CHARBUF | chaining of the container ID and the directory name. Useful when trying to identify which container a directory belongs to.
`fd.proto` | PORT | matches the protocol (either client or server) of the fd.
`fd.cproto` | CHARBUF | for TCP/UDP FDs, the client protocol.
`fd.sproto` | CHARBUF | for TCP/UDP FDs, server protocol.
`fd.lproto` | CHARBUF | for TCP/UDP FDs, the local protocol.
`fd.rproto` | CHARBUF | for TCP/UDP FDs, the remote protocol.
`fd.net` | IPNET | matches the IP network (client or server) of the fd.
`fd.cnet` | IPNET | matches the client IP network of the fd.
`fd.snet` | IPNET | matches the server IP network of the fd.
`fd.lnet` | IPNET | matches the local IP network of the fd.
`fd.rnet` | IPNET | matches the remote IP network of the fd.
`fd.connected` | BOOL | for TCP/UDP FDs, 'true' if the socket is connected.
`fd.name_changed` | BOOL | True when an event changes the name of an fd used by this event. This can occur in some cases such as udp connections where the connection tuple changes.
`fd.cip.name` | CHARBUF | Domain name associated with the client IP address.
`fd.sip.name` | CHARBUF | Domain name associated with the server IP address.
`fd.lip.name` | CHARBUF | Domain name associated with the local IP address.
`fd.rip.name` | CHARBUF | Domain name associated with the remote IP address.
`fd.dev` | INT32 | device number (major/minor) containing the referenced file
`fd.dev.major` | INT32 | major device number containing the referenced file
`fd.dev.minor` | INT32 | minor device number containing the referenced file
`fd.ino` | INT64 | inode number of the referenced file
`fd.nameraw` | CHARBUF | FD full name raw. Just like fd.name, but only used if fd is a file path. File path is kept raw with limited sanitization and without deriving the absolute path.
`fd.types` | CHARBUF | List of FD types in used. Can be passed an fd number e.g. fd.types[0] to get the type of stdout as a single item list.

## Field Class: fs.path

Every syscall that has a filesystem path in its arguments has these fields set with information related to the path arguments. This differs from the fd.* fields as it includes syscalls like unlink, rename, etc. that act directly on filesystem paths as compared to opened file descriptors.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`fs.path.name` | CHARBUF | For any event type that deals with a filesystem path, the path the file syscall is operating on. This path is always fully resolved, prepending the thread cwd when needed.
`fs.path.nameraw` | CHARBUF | For any event type that deals with a filesystem path, the path the file syscall is operating on. This path is always the path provided to the syscall and may not be fully resolved.
`fs.path.source` | CHARBUF | For any event type that deals with a filesystem path, and specifically for a source and target like mv, cp, etc, the source path the file syscall is operating on. This path is always fully resolved, prepending the thread cwd when needed.
`fs.path.sourceraw` | CHARBUF | For any event type that deals with a filesystem path, and specifically for a source and target like mv, cp, etc, the source path the file syscall is operating on. This path is always the path provided to the syscall and may not be fully resolved.
`fs.path.target` | CHARBUF | For any event type that deals with a filesystem path, and specifically for a target and target like mv, cp, etc, the target path the file syscall is operating on. This path is always fully resolved, prepending the thread cwd when needed.
`fs.path.targetraw` | CHARBUF | For any event type that deals with a filesystem path, and specifically for a target and target like mv, cp, etc, the target path the file syscall is operating on. This path is always the path provided to the syscall and may not be fully resolved.

## Field Class: syslog

Content of Syslog messages.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`syslog.facility.str` | CHARBUF | facility as a string.
`syslog.facility` | UINT32 | facility as a number (0-23).
`syslog.severity.str` | CHARBUF | severity as a string. Can have one of these values: emerg, alert, crit, err, warn, notice, info, debug
`syslog.severity` | UINT32 | severity as a number (0-7).
`syslog.message` | CHARBUF | message sent to syslog.

## Field Class: fdlist

Poll event related fields.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`fdlist.nums` | CHARBUF | for poll events, this is a comma-separated list of the FD numbers in the 'fds' argument, returned as a string.
`fdlist.names` | CHARBUF | for poll events, this is a comma-separated list of the FD names in the 'fds' argument, returned as a string.
`fdlist.cips` | CHARBUF | for poll events, this is a comma-separated list of the client IP addresses in the 'fds' argument, returned as a string.
`fdlist.sips` | CHARBUF | for poll events, this is a comma-separated list of the server IP addresses in the 'fds' argument, returned as a string.
`fdlist.cports` | CHARBUF | for TCP/UDP FDs, for poll events, this is a comma-separated list of the client TCP/UDP ports in the 'fds' argument, returned as a string.
`fdlist.sports` | CHARBUF | for poll events, this is a comma-separated list of the server TCP/UDP ports in the 'fds' argument, returned as a string.

## Field Class: k8s

Kubernetes context about pods and namespace name. These fields are populated with data gathered from the container runtime.

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`k8s.ns.name` | CHARBUF | The Kubernetes namespace name. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.name` | CHARBUF | The Kubernetes pod name. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.id` | CHARBUF | [LEGACY] The Kubernetes pod UID, e.g. 3e41dc6b-08a8-44db-bc2a-3724b18ab19a. This legacy field points to `k8s.pod.uid`; however, the pod ID typically refers to the pod sandbox ID. We recommend using the semantically more accurate `k8s.pod.uid` field. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.uid` | CHARBUF | The Kubernetes pod UID, e.g. 3e41dc6b-08a8-44db-bc2a-3724b18ab19a. Note that the pod UID is a unique identifier assigned upon pod creation within Kubernetes, allowing the Kubernetes control plane to manage and track pods reliably. As such, it is fundamentally a different concept compared to the pod sandbox ID. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.sandbox_id` | CHARBUF | The truncated Kubernetes pod sandbox ID (first 12 characters), e.g 63060edc2d3a. The sandbox ID is specific to the container runtime environment. It is the equivalent of the container ID for the pod / sandbox and extracted from the Linux cgroups. As such, it differs from the pod UID. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet. In Kubernetes, pod sandbox container processes can exist where `container.id` matches `k8s.pod.sandbox_id`, lacking other 'container.*' details.
`k8s.pod.full_sandbox_id` | CHARBUF | The full Kubernetes pod / sandbox ID, e.g 63060edc2d3aa803ab559f2393776b151f99fc5b05035b21db66b3b62246ad6a. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.label` | CHARBUF | The Kubernetes pod label. The label can be accessed either with the familiar brackets notation, e.g. 'k8s.pod.label[foo]' or by appending a dot followed by the name, e.g. 'k8s.pod.label.foo'. The label name itself can include the original special characters such as '.', '-', '_' or '/' characters. For instance, 'k8s.pod.label[app.kubernetes.io/name]', 'k8s.pod.label.app.kubernetes.io/name' or 'k8s.pod.label[custom-label_one]' are all valid. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.labels` | CHARBUF | The Kubernetes pod comma-separated key/value labels. E.g. 'foo1:bar1,foo2:bar2'. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.ip` | CHARBUF | The Kubernetes pod ip, same as container.ip field as each container in a pod shares the network stack of the sandbox / pod. Only ipv4 addresses are tracked. Consider k8s.pod.cni.json for logging ip addresses for each network interface. This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.
`k8s.pod.cni.json` | CHARBUF | The Kubernetes pod CNI result field from the respective pod status info, same as container.cni.json field. It contains ip addresses for each network interface exposed as unparsed escaped JSON string. Supported for CRI container engine (containerd, cri-o runtimes), optimized for containerd (some non-critical JSON keys removed). Useful for tracking ips (ipv4 and ipv6, dual-stack support) for each network interface (multi-interface support). This field is extracted from the container runtime socket simultaneously as we look up the 'container.*' fields. In cases of lookup delays, it may not be available yet.

## Field Class: security-dns (plugin)

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`dns.domain` | CHARBUF | The domain being queried (e.g. sysdig.com) as a string.
`dns.query_type` | CHARBUF | The type of lookup (e.g. A, AAAA, CNAME) of the query as a string.
`dns.query_class` | CHARBUF | The class of lookup (e.g. IN) as a string.
`dns.success` | BOOL | Whether the query was successful or not as a boolean value.
`dns.type` | CHARBUF | The type of DNS event as a string, either "query" or "response".
`dns.result` | UINT64 | The result code (RCODE) of the query, 0 on success, see RFC-1035 for other values.
`dns.truncated` | BOOL | Whether or not the query was truncated as a boolean value.
`dns.query.domains` | CHARBUF | A list of all the domains being queried as strings.
`dns.query.domain` | CHARBUF | An indexed field for the domain being queried as a string.
`dns.query.type` | CHARBUF | An indexed field for the type of query (e.g. A, AAAA, CNAME) being queried as a string.
`dns.query.class` | CHARBUF | An indexed field for the class of query (e.g. IN) being queried as a string.
`dns.query.lengths` | UINT64 | The total length of the string of each domain being looked up.
`dns.query.length` | UINT64 | An indexed field for the length of the domain string in each query.
`dns.response.domains` | CHARBUF | A list of all the domains in the response as strings.
`dns.response.domain` | CHARBUF | An indexed field for each domain in the response as a string.
`dns.response.ttl` | UINT64 | An indexed field for The Time To Live of the record as an integer
`dns.response.type` | CHARBUF | An indexed field for the type of respose (e.g. A, AAAA, CNAME) as a string. This can differ from what was originally queried.
`dns.response.class` | CHARBUF | An indexed field for the class of respose (e.g. IN) as a string.
`dns.response.values` | CHARBUF | A list containing the value of each response as a string.
`dns.response.value` | CHARBUF | An indexed field for the class of respose (e.g. IN) as a string.
`dns.response.cnames` | CHARBUF | A list of all the CNAMES in the response as strings. This will be empty if no CNAME records are present.
`dns.response.cname` | CHARBUF | An indexed field for CNAME response records. This will be empty if the given index is not a CNAME.
`dns.response.txts` | CHARBUF | A list of all the TXT records in the response as strings. This will be empty if no TXT records are present.
`dns.response.txt` | CHARBUF | An indexed field for TXT response records. This will be empty if the given index is not a TXT record.
`dns.response.srvs` | CHARBUF | A list of all the SRV records in the response as strings. This will be empty if no SRV records are present.
`dns.response.srv` | CHARBUF | An indexed field for SRV response records. This will be empty if the given index is not an SRV record.
`dns.response.ips` | IPADDR | List of IP addresses in the response.
`dns.response.ip` | IPADDR | An indexed field for ip address (A, AAAA) response records. This will be empty if the given index is not an A or AAAA record.
`dns.server_ip` | IPADDR | The ip address of the DNS server.
`connect.domains` | CHARBUF | Domain names which map to fd.sip in the connect syscall

## Field Class: security-hashing (plugin)

Event Sources: syscall

Name | Type | Description
:----|:-----|:-----------
`proc.hash.sha256` | CHARBUF | The hash of the file executed by this process
`proc.hash.has_match` | BOOL | Whether or not the hash of the file beeing executed by this process has a match in the hash database
`proc.hash.category` | CHARBUF | In case proc.has_match is true, the category of the malware being executed

## Syscall events

Default | Dir | Name | Params
:-------|:----|:-----|:-----
Yes | `>` | `open` | FSPATH **name**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, UINT32 **mode**
Yes | `<` | `open` | FD **fd**, FSPATH **name**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, UINT32 **mode**, UINT32 **dev**, UINT64 **ino**
Yes | `>` | `close` | FD **fd**
Yes | `<` | `close` | ERRNO **res**
No | `>` | `read` | FD **fd**, UINT32 **size**
No | `<` | `read` | ERRNO **res**, BYTEBUF **data**
No | `>` | `write` | FD **fd**, UINT32 **size**
No | `<` | `write` | ERRNO **res**, BYTEBUF **data**
Yes | `>` | `socket` | ENUMFLAGS32 **domain**: *AF_NFC*, *AF_ALG*, *AF_CAIF*, *AF_IEEE802154*, *AF_PHONET*, *AF_ISDN*, *AF_RXRPC*, *AF_IUCV*, *AF_BLUETOOTH*, *AF_TIPC*, *AF_CAN*, *AF_LLC*, *AF_WANPIPE*, *AF_PPPOX*, *AF_IRDA*, *AF_SNA*, *AF_RDS*, *AF_ATMSVC*, *AF_ECONET*, *AF_ASH*, *AF_PACKET*, *AF_ROUTE*, *AF_NETLINK*, *AF_KEY*, *AF_SECURITY*, *AF_NETBEUI*, *AF_DECnet*, *AF_ROSE*, *AF_INET6*, *AF_X25*, *AF_ATMPVC*, *AF_BRIDGE*, *AF_NETROM*, *AF_APPLETALK*, *AF_IPX*, *AF_AX25*, *AF_INET*, *AF_LOCAL*, *AF_UNIX*, *AF_UNSPEC*, UINT32 **type**, UINT32 **proto**
Yes | `<` | `socket` | FD **fd**
Yes | `>` | `bind` | FD **fd**
Yes | `<` | `bind` | ERRNO **res**, SOCKADDR **addr**
Yes | `>` | `connect` | FD **fd**, SOCKADDR **addr**
Yes | `<` | `connect` | ERRNO **res**, SOCKTUPLE **tuple**, FD **fd**
Yes | `>` | `listen` | FD **fd**, INT32 **backlog**
Yes | `<` | `listen` | ERRNO **res**
No | `>` | `send` | FD **fd**, UINT32 **size**
No | `<` | `send` | ERRNO **res**, BYTEBUF **data**
Yes | `>` | `sendto` | FD **fd**, UINT32 **size**, SOCKTUPLE **tuple**
Yes | `<` | `sendto` | ERRNO **res**, BYTEBUF **data**
No | `>` | `recv` | FD **fd**, UINT32 **size**
No | `<` | `recv` | ERRNO **res**, BYTEBUF **data**
Yes | `>` | `recvfrom` | FD **fd**, UINT32 **size**
Yes | `<` | `recvfrom` | ERRNO **res**, BYTEBUF **data**, SOCKTUPLE **tuple**
Yes | `>` | `shutdown` | FD **fd**, ENUMFLAGS8 **how**: *SHUT_UNKNOWN*, *SHUT_RDWR*, *SHUT_WR*, *SHUT_RD*
Yes | `<` | `shutdown` | ERRNO **res**
Yes | `>` | `getsockname` |
Yes | `<` | `getsockname` |
Yes | `>` | `getpeername` |
Yes | `<` | `getpeername` |
Yes | `>` | `socketpair` | ENUMFLAGS32 **domain**: *AF_NFC*, *AF_ALG*, *AF_CAIF*, *AF_IEEE802154*, *AF_PHONET*, *AF_ISDN*, *AF_RXRPC*, *AF_IUCV*, *AF_BLUETOOTH*, *AF_TIPC*, *AF_CAN*, *AF_LLC*, *AF_WANPIPE*, *AF_PPPOX*, *AF_IRDA*, *AF_SNA*, *AF_RDS*, *AF_ATMSVC*, *AF_ECONET*, *AF_ASH*, *AF_PACKET*, *AF_ROUTE*, *AF_NETLINK*, *AF_KEY*, *AF_SECURITY*, *AF_NETBEUI*, *AF_DECnet*, *AF_ROSE*, *AF_INET6*, *AF_X25*, *AF_ATMPVC*, *AF_BRIDGE*, *AF_NETROM*, *AF_APPLETALK*, *AF_IPX*, *AF_AX25*, *AF_INET*, *AF_LOCAL*, *AF_UNIX*, *AF_UNSPEC*, UINT32 **type**, UINT32 **proto**
Yes | `<` | `socketpair` | ERRNO **res**, FD **fd1**, FD **fd2**, UINT64 **source**, UINT64 **peer**
Yes | `>` | `setsockopt` |
Yes | `<` | `setsockopt` | ERRNO **res**, FD **fd**, ENUMFLAGS8 **level**: *SOL_SOCKET*, *SOL_TCP*, *UNKNOWN*, ENUMFLAGS8 **optname**: *SO_COOKIE*, *SO_MEMINFO*, *SO_PEERGROUPS*, *SO_ATTACH_BPF*, *SO_INCOMING_CPU*, *SO_BPF_EXTENSIONS*, *SO_MAX_PACING_RATE*, *SO_BUSY_POLL*, *SO_SELECT_ERR_QUEUE*, *SO_LOCK_FILTER*, *SO_NOFCS*, *SO_PEEK_OFF*, *SO_WIFI_STATUS*, *SO_RXQ_OVFL*, *SO_DOMAIN*, *SO_PROTOCOL*, *SO_TIMESTAMPING*, *SO_MARK*, *SO_TIMESTAMPNS*, *SO_PASSSEC*, *SO_PEERSEC*, *SO_ACCEPTCONN*, *SO_TIMESTAMP*, *SO_PEERNAME*, *SO_DETACH_FILTER*, *SO_ATTACH_FILTER*, *SO_BINDTODEVICE*, *SO_SECURITY_ENCRYPTION_NETWORK*, *SO_SECURITY_ENCRYPTION_TRANSPORT*, *SO_SECURITY_AUTHENTICATION*, *SO_SNDTIMEO*, *SO_RCVTIMEO*, *SO_SNDLOWAT*, *SO_RCVLOWAT*, *SO_PEERCRED*, *SO_PASSCRED*, *SO_REUSEPORT*, *SO_BSDCOMPAT*, *SO_LINGER*, *SO_PRIORITY*, *SO_NO_CHECK*, *SO_OOBINLINE*, *SO_KEEPALIVE*, *SO_RCVBUFFORCE*, *SO_SNDBUFFORCE*, *SO_RCVBUF*, *SO_SNDBUF*, *SO_BROADCAST*, *SO_DONTROUTE*, *SO_ERROR*, *SO_TYPE*, *SO_REUSEADDR*, *SO_DEBUG*, *UNKNOWN*, DYNAMIC **val**, UINT32 **optlen**
Yes | `>` | `getsockopt` |
Yes | `<` | `getsockopt` | ERRNO **res**, FD **fd**, ENUMFLAGS8 **level**: *SOL_SOCKET*, *SOL_TCP*, *UNKNOWN*, ENUMFLAGS8 **optname**: *SO_COOKIE*, *SO_MEMINFO*, *SO_PEERGROUPS*, *SO_ATTACH_BPF*, *SO_INCOMING_CPU*, *SO_BPF_EXTENSIONS*, *SO_MAX_PACING_RATE*, *SO_BUSY_POLL*, *SO_SELECT_ERR_QUEUE*, *SO_LOCK_FILTER*, *SO_NOFCS*, *SO_PEEK_OFF*, *SO_WIFI_STATUS*, *SO_RXQ_OVFL*, *SO_DOMAIN*, *SO_PROTOCOL*, *SO_TIMESTAMPING*, *SO_MARK*, *SO_TIMESTAMPNS*, *SO_PASSSEC*, *SO_PEERSEC*, *SO_ACCEPTCONN*, *SO_TIMESTAMP*, *SO_PEERNAME*, *SO_DETACH_FILTER*, *SO_ATTACH_FILTER*, *SO_BINDTODEVICE*, *SO_SECURITY_ENCRYPTION_NETWORK*, *SO_SECURITY_ENCRYPTION_TRANSPORT*, *SO_SECURITY_AUTHENTICATION*, *SO_SNDTIMEO*, *SO_RCVTIMEO*, *SO_SNDLOWAT*, *SO_RCVLOWAT*, *SO_PEERCRED*, *SO_PASSCRED*, *SO_REUSEPORT*, *SO_BSDCOMPAT*, *SO_LINGER*, *SO_PRIORITY*, *SO_NO_CHECK*, *SO_OOBINLINE*, *SO_KEEPALIVE*, *SO_RCVBUFFORCE*, *SO_SNDBUFFORCE*, *SO_RCVBUF*, *SO_SNDBUF*, *SO_BROADCAST*, *SO_DONTROUTE*, *SO_ERROR*, *SO_TYPE*, *SO_REUSEADDR*, *SO_DEBUG*, *UNKNOWN*, DYNAMIC **val**, UINT32 **optlen**
Yes | `>` | `sendmsg` | FD **fd**, UINT32 **size**, SOCKTUPLE **tuple**
Yes | `<` | `sendmsg` | ERRNO **res**, BYTEBUF **data**
No | `>` | `sendmmsg` |
No | `<` | `sendmmsg` |
Yes | `>` | `recvmsg` | FD **fd**
Yes | `<` | `recvmsg` | ERRNO **res**, UINT32 **size**, BYTEBUF **data**, SOCKTUPLE **tuple**, BYTEBUF **msgcontrol**
No | `>` | `recvmmsg` |
No | `<` | `recvmmsg` |
Yes | `>` | `creat` | FSPATH **name**, UINT32 **mode**
Yes | `<` | `creat` | FD **fd**, FSPATH **name**, UINT32 **mode**, UINT32 **dev**, UINT64 **ino**
Yes | `>` | `pipe` |
Yes | `<` | `pipe` | ERRNO **res**, FD **fd1**, FD **fd2**, UINT64 **ino**
Yes | `>` | `eventfd` | UINT64 **initval**, UINT32 **flags**
Yes | `<` | `eventfd` | FD **res**
Yes | `>` | `futex` | UINT64 **addr**, ENUMFLAGS16 **op**: *FUTEX_CLOCK_REALTIME*, *FUTEX_PRIVATE_FLAG*, *FUTEX_CMP_REQUEUE_PI*, *FUTEX_WAIT_REQUEUE_PI*, *FUTEX_WAKE_BITSET*, *FUTEX_WAIT_BITSET*, *FUTEX_TRYLOCK_PI*, *FUTEX_UNLOCK_PI*, *FUTEX_LOCK_PI*, *FUTEX_WAKE_OP*, *FUTEX_CMP_REQUEUE*, *FUTEX_REQUEUE*, *FUTEX_FD*, *FUTEX_WAKE*, *FUTEX_WAIT*, UINT64 **val**
Yes | `<` | `futex` | ERRNO **res**
Yes | `>` | `stat` |
Yes | `<` | `stat` | ERRNO **res**, FSPATH **path**
Yes | `>` | `lstat` |
Yes | `<` | `lstat` | ERRNO **res**, FSPATH **path**
Yes | `>` | `fstat` | FD **fd**
Yes | `<` | `fstat` | ERRNO **res**
Yes | `>` | `stat64` |
Yes | `<` | `stat64` | ERRNO **res**, FSPATH **path**
Yes | `>` | `lstat64` |
Yes | `<` | `lstat64` | ERRNO **res**, FSPATH **path**
Yes | `>` | `fstat64` | FD **fd**
Yes | `<` | `fstat64` | ERRNO **res**
Yes | `>` | `epoll_wait` | ERRNO **maxevents**
Yes | `<` | `epoll_wait` | ERRNO **res**
Yes | `>` | `poll` | FDLIST **fds**, INT64 **timeout**
Yes | `<` | `poll` | ERRNO **res**, FDLIST **fds**
Yes | `>` | `select` |
Yes | `<` | `select` | ERRNO **res**
Yes | `>` | `lseek` | FD **fd**, UINT64 **offset**, ENUMFLAGS8 **whence**: *SEEK_END*, *SEEK_CUR*, *SEEK_SET*
Yes | `<` | `lseek` | ERRNO **res**
Yes | `>` | `llseek` | FD **fd**, UINT64 **offset**, ENUMFLAGS8 **whence**: *SEEK_END*, *SEEK_CUR*, *SEEK_SET*
Yes | `<` | `llseek` | ERRNO **res**
Yes | `>` | `getcwd` |
Yes | `<` | `getcwd` | ERRNO **res**, CHARBUF **path**
Yes | `>` | `chdir` |
Yes | `<` | `chdir` | ERRNO **res**, CHARBUF **path**
Yes | `>` | `fchdir` | FD **fd**
Yes | `<` | `fchdir` | ERRNO **res**
No | `>` | `pread` | FD **fd**, UINT32 **size**, UINT64 **pos**
No | `<` | `pread` | ERRNO **res**, BYTEBUF **data**
No | `>` | `pwrite` | FD **fd**, UINT32 **size**, UINT64 **pos**
No | `<` | `pwrite` | ERRNO **res**, BYTEBUF **data**
No | `>` | `readv` | FD **fd**
No | `<` | `readv` | ERRNO **res**, UINT32 **size**, BYTEBUF **data**
No | `>` | `writev` | FD **fd**, UINT32 **size**
No | `<` | `writev` | ERRNO **res**, BYTEBUF **data**
No | `>` | `preadv` | FD **fd**, UINT64 **pos**
No | `<` | `preadv` | ERRNO **res**, UINT32 **size**, BYTEBUF **data**
No | `>` | `pwritev` | FD **fd**, UINT32 **size**, UINT64 **pos**
No | `<` | `pwritev` | ERRNO **res**, BYTEBUF **data**
Yes | `>` | `signalfd` | FD **fd**, UINT32 **mask**, UINT8 **flags**
Yes | `<` | `signalfd` | FD **res**
Yes | `>` | `kill` | PID **pid**, SIGTYPE **sig**
Yes | `<` | `kill` | ERRNO **res**
Yes | `>` | `tkill` | PID **tid**, SIGTYPE **sig**
Yes | `<` | `tkill` | ERRNO **res**
Yes | `>` | `tgkill` | PID **pid**, PID **tid**, SIGTYPE **sig**
Yes | `<` | `tgkill` | ERRNO **res**
Yes | `>` | `nanosleep` | RELTIME **interval**
Yes | `<` | `nanosleep` | ERRNO **res**
Yes | `>` | `timerfd_create` | UINT8 **clockid**, UINT8 **flags**
Yes | `<` | `timerfd_create` | FD **res**
Yes | `>` | `inotify_init` | UINT8 **flags**
Yes | `<` | `inotify_init` | FD **res**
Yes | `>` | `getrlimit` | ENUMFLAGS8 **resource**: *RLIMIT_UNKNOWN*, *RLIMIT_RTTIME*, *RLIMIT_RTPRIO*, *RLIMIT_NICE*, *RLIMIT_MSGQUEUE*, *RLIMIT_SIGPENDING*, *RLIMIT_LOCKS*, *RLIMIT_AS*, *RLIMIT_MEMLOCK*, *RLIMIT_NOFILE*, *RLIMIT_NPROC*, *RLIMIT_RSS*, *RLIMIT_CORE*, *RLIMIT_STACK*, *RLIMIT_DATA*, *RLIMIT_FSIZE*, *RLIMIT_CPU*
Yes | `<` | `getrlimit` | ERRNO **res**, INT64 **cur**, INT64 **max**
Yes | `>` | `setrlimit` | ENUMFLAGS8 **resource**: *RLIMIT_UNKNOWN*, *RLIMIT_RTTIME*, *RLIMIT_RTPRIO*, *RLIMIT_NICE*, *RLIMIT_MSGQUEUE*, *RLIMIT_SIGPENDING*, *RLIMIT_LOCKS*, *RLIMIT_AS*, *RLIMIT_MEMLOCK*, *RLIMIT_NOFILE*, *RLIMIT_NPROC*, *RLIMIT_RSS*, *RLIMIT_CORE*, *RLIMIT_STACK*, *RLIMIT_DATA*, *RLIMIT_FSIZE*, *RLIMIT_CPU*
Yes | `<` | `setrlimit` | ERRNO **res**, INT64 **cur**, INT64 **max**, ENUMFLAGS8 **resource**: *RLIMIT_UNKNOWN*, *RLIMIT_RTTIME*, *RLIMIT_RTPRIO*, *RLIMIT_NICE*, *RLIMIT_MSGQUEUE*, *RLIMIT_SIGPENDING*, *RLIMIT_LOCKS*, *RLIMIT_AS*, *RLIMIT_MEMLOCK*, *RLIMIT_NOFILE*, *RLIMIT_NPROC*, *RLIMIT_RSS*, *RLIMIT_CORE*, *RLIMIT_STACK*, *RLIMIT_DATA*, *RLIMIT_FSIZE*, *RLIMIT_CPU*
Yes | `>` | `prlimit` | PID **pid**, ENUMFLAGS8 **resource**: *RLIMIT_UNKNOWN*, *RLIMIT_RTTIME*, *RLIMIT_RTPRIO*, *RLIMIT_NICE*, *RLIMIT_MSGQUEUE*, *RLIMIT_SIGPENDING*, *RLIMIT_LOCKS*, *RLIMIT_AS*, *RLIMIT_MEMLOCK*, *RLIMIT_NOFILE*, *RLIMIT_NPROC*, *RLIMIT_RSS*, *RLIMIT_CORE*, *RLIMIT_STACK*, *RLIMIT_DATA*, *RLIMIT_FSIZE*, *RLIMIT_CPU*
Yes | `<` | `prlimit` | ERRNO **res**, INT64 **newcur**, INT64 **newmax**, INT64 **oldcur**, INT64 **oldmax**, INT64 **pid**, ENUMFLAGS8 **resource**: *RLIMIT_UNKNOWN*, *RLIMIT_RTTIME*, *RLIMIT_RTPRIO*, *RLIMIT_NICE*, *RLIMIT_MSGQUEUE*, *RLIMIT_SIGPENDING*, *RLIMIT_LOCKS*, *RLIMIT_AS*, *RLIMIT_MEMLOCK*, *RLIMIT_NOFILE*, *RLIMIT_NPROC*, *RLIMIT_RSS*, *RLIMIT_CORE*, *RLIMIT_STACK*, *RLIMIT_DATA*, *RLIMIT_FSIZE*, *RLIMIT_CPU*
Yes | `>` | `fcntl` | FD **fd**, ENUMFLAGS8 **cmd**: *F_GETPIPE_SZ*, *F_SETPIPE_SZ*, *F_NOTIFY*, *F_DUPFD_CLOEXEC*, *F_CANCELLK*, *F_GETLEASE*, *F_SETLEASE*, *F_GETOWN_EX*, *F_SETOWN_EX*, *F_SETLKW64*, *F_SETLK64*, *F_GETLK64*, *F_GETSIG*, *F_SETSIG*, *F_GETOWN*, *F_SETOWN*, *F_SETLKW*, *F_SETLK*, *F_GETLK*, *F_SETFL*, *F_GETFL*, *F_SETFD*, *F_GETFD*, *F_DUPFD*, *F_OFD_GETLK*, *F_OFD_SETLK*, *F_OFD_SETLKW*, *UNKNOWN*
Yes | `<` | `fcntl` | FD **res**, FD **fd**, ENUMFLAGS8 **cmd**: *F_GETPIPE_SZ*, *F_SETPIPE_SZ*, *F_NOTIFY*, *F_DUPFD_CLOEXEC*, *F_CANCELLK*, *F_GETLEASE*, *F_SETLEASE*, *F_GETOWN_EX*, *F_SETOWN_EX*, *F_SETLKW64*, *F_SETLK64*, *F_GETLK64*, *F_GETSIG*, *F_SETSIG*, *F_GETOWN*, *F_SETOWN*, *F_SETLKW*, *F_SETLK*, *F_GETLK*, *F_SETFL*, *F_GETFL*, *F_SETFD*, *F_GETFD*, *F_DUPFD*, *F_OFD_GETLK*, *F_OFD_SETLK*, *F_OFD_SETLKW*, *UNKNOWN*
Yes | `>` | `brk` | UINT64 **addr**
Yes | `<` | `brk` | UINT64 **res**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**
Yes | `>` | `mmap` | UINT64 **addr**, UINT64 **length**, FLAGS32 **prot**: *PROT_READ*, *PROT_WRITE*, *PROT_EXEC*, *PROT_SEM*, *PROT_GROWSDOWN*, *PROT_GROWSUP*, *PROT_SAO*, *PROT_NONE*, FLAGS32 **flags**: *MAP_SHARED*, *MAP_PRIVATE*, *MAP_FIXED*, *MAP_ANONYMOUS*, *MAP_32BIT*, *MAP_RENAME*, *MAP_NORESERVE*, *MAP_POPULATE*, *MAP_NONBLOCK*, *MAP_GROWSDOWN*, *MAP_DENYWRITE*, *MAP_EXECUTABLE*, *MAP_INHERIT*, *MAP_FILE*, *MAP_LOCKED*, FD **fd**, UINT64 **offset**
Yes | `<` | `mmap` | ERRNO **res**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**
Yes | `>` | `mmap2` | UINT64 **addr**, UINT64 **length**, FLAGS32 **prot**: *PROT_READ*, *PROT_WRITE*, *PROT_EXEC*, *PROT_SEM*, *PROT_GROWSDOWN*, *PROT_GROWSUP*, *PROT_SAO*, *PROT_NONE*, FLAGS32 **flags**: *MAP_SHARED*, *MAP_PRIVATE*, *MAP_FIXED*, *MAP_ANONYMOUS*, *MAP_32BIT*, *MAP_RENAME*, *MAP_NORESERVE*, *MAP_POPULATE*, *MAP_NONBLOCK*, *MAP_GROWSDOWN*, *MAP_DENYWRITE*, *MAP_EXECUTABLE*, *MAP_INHERIT*, *MAP_FILE*, *MAP_LOCKED*, FD **fd**, UINT64 **pgoffset**
Yes | `<` | `mmap2` | ERRNO **res**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**
Yes | `>` | `munmap` | UINT64 **addr**, UINT64 **length**
Yes | `<` | `munmap` | ERRNO **res**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**
Yes | `>` | `splice` | FD **fd_in**, FD **fd_out**, UINT64 **size**, FLAGS32 **flags**: *SPLICE_F_MOVE*, *SPLICE_F_NONBLOCK*, *SPLICE_F_MORE*, *SPLICE_F_GIFT*
Yes | `<` | `splice` | ERRNO **res**
Yes | `>` | `ptrace` | ENUMFLAGS16 **request**: *PTRACE_SINGLEBLOCK*, *PTRACE_SYSEMU_SINGLESTEP*, *PTRACE_SYSEMU*, *PTRACE_ARCH_PRCTL*, *PTRACE_SET_THREAD_AREA*, *PTRACE_GET_THREAD_AREA*, *PTRACE_OLDSETOPTIONS*, *PTRACE_SETFPXREGS*, *PTRACE_GETFPXREGS*, *PTRACE_SETFPREGS*, *PTRACE_GETFPREGS*, *PTRACE_SETREGS*, *PTRACE_GETREGS*, *PTRACE_SETSIGMASK*, *PTRACE_GETSIGMASK*, *PTRACE_PEEKSIGINFO*, *PTRACE_LISTEN*, *PTRACE_INTERRUPT*, *PTRACE_SEIZE*, *PTRACE_SETREGSET*, *PTRACE_GETREGSET*, *PTRACE_SETSIGINFO*, *PTRACE_GETSIGINFO*, *PTRACE_GETEVENTMSG*, *PTRACE_SETOPTIONS*, *PTRACE_SYSCALL*, *PTRACE_DETACH*, *PTRACE_ATTACH*, *PTRACE_SINGLESTEP*, *PTRACE_KILL*, *PTRACE_CONT*, *PTRACE_POKEUSR*, *PTRACE_POKEDATA*, *PTRACE_POKETEXT*, *PTRACE_PEEKUSR*, *PTRACE_PEEKDATA*, *PTRACE_PEEKTEXT*, *PTRACE_TRACEME*, *PTRACE_UNKNOWN*, PID **pid**
Yes | `<` | `ptrace` | ERRNO **res**, DYNAMIC **addr**, DYNAMIC **data**
Yes | `>` | `ioctl` | FD **fd**, UINT64 **request**, UINT64 **argument**
Yes | `<` | `ioctl` | ERRNO **res**
Yes | `>` | `rename` |
Yes | `<` | `rename` | ERRNO **res**, FSPATH **oldpath**, FSPATH **newpath**
Yes | `>` | `renameat` |
Yes | `<` | `renameat` | ERRNO **res**, FD **olddirfd**, FSRELPATH **oldpath**, FD **newdirfd**, FSRELPATH **newpath**
Yes | `>` | `symlink` |
Yes | `<` | `symlink` | ERRNO **res**, CHARBUF **target**, FSPATH **linkpath**
Yes | `>` | `symlinkat` |
Yes | `<` | `symlinkat` | ERRNO **res**, CHARBUF **target**, FD **linkdirfd**, FSRELPATH **linkpath**
No | `>` | `sendfile` | FD **out_fd**, FD **in_fd**, UINT64 **offset**, UINT64 **size**
No | `<` | `sendfile` | ERRNO **res**, UINT64 **offset**
Yes | `>` | `quotactl` | FLAGS16 **cmd**: *Q_QUOTAON*, *Q_QUOTAOFF*, *Q_GETFMT*, *Q_GETINFO*, *Q_SETINFO*, *Q_GETQUOTA*, *Q_SETQUOTA*, *Q_SYNC*, *Q_XQUOTAON*, *Q_XQUOTAOFF*, *Q_XGETQUOTA*, *Q_XSETQLIM*, *Q_XGETQSTAT*, *Q_XQUOTARM*, *Q_XQUOTASYNC*, FLAGS8 **type**: *USRQUOTA*, *GRPQUOTA*, UINT32 **id**, FLAGS8 **quota_fmt**: *QFMT_NOT_USED*, *QFMT_VFS_OLD*, *QFMT_VFS_V0*, *QFMT_VFS_V1*
Yes | `<` | `quotactl` | ERRNO **res**, CHARBUF **special**, CHARBUF **quotafilepath**, UINT64 **dqb_bhardlimit**, UINT64 **dqb_bsoftlimit**, UINT64 **dqb_curspace**, UINT64 **dqb_ihardlimit**, UINT64 **dqb_isoftlimit**, RELTIME **dqb_btime**, RELTIME **dqb_itime**, RELTIME **dqi_bgrace**, RELTIME **dqi_igrace**, FLAGS8 **dqi_flags**: *DQF_NONE*, *V1_DQF_RSQUASH*, FLAGS8 **quota_fmt_out**: *QFMT_NOT_USED*, *QFMT_VFS_OLD*, *QFMT_VFS_V0*, *QFMT_VFS_V1*
Yes | `>` | `setresuid` | UID **ruid**, UID **euid**, UID **suid**
Yes | `<` | `setresuid` | ERRNO **res**
Yes | `>` | `setresgid` | GID **rgid**, GID **egid**, GID **sgid**
Yes | `<` | `setresgid` | ERRNO **res**
Yes | `>` | `setuid` | UID **uid**
Yes | `<` | `setuid` | ERRNO **res**
Yes | `>` | `setgid` | GID **gid**
Yes | `<` | `setgid` | ERRNO **res**
Yes | `>` | `getuid` |
Yes | `<` | `getuid` | UID **uid**
Yes | `>` | `geteuid` |
Yes | `<` | `geteuid` | UID **euid**
Yes | `>` | `getgid` |
Yes | `<` | `getgid` | GID **gid**
Yes | `>` | `getegid` |
Yes | `<` | `getegid` | GID **egid**
Yes | `>` | `getresuid` |
Yes | `<` | `getresuid` | ERRNO **res**, UID **ruid**, UID **euid**, UID **suid**
Yes | `>` | `getresgid` |
Yes | `<` | `getresgid` | ERRNO **res**, GID **rgid**, GID **egid**, GID **sgid**
Yes | `>` | `clone` |
Yes | `<` | `clone` | PID **res**, CHARBUF **exe**, BYTEBUF **args**, PID **tid**, PID **pid**, PID **ptid**, CHARBUF **cwd**, INT64 **fdlimit**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**, CHARBUF **comm**, BYTEBUF **cgroups**, FLAGS32 **flags**: *CLONE_FILES*, *CLONE_FS*, *CLONE_IO*, *CLONE_NEWIPC*, *CLONE_NEWNET*, *CLONE_NEWNS*, *CLONE_NEWPID*, *CLONE_NEWUTS*, *CLONE_PARENT*, *CLONE_PARENT_SETTID*, *CLONE_PTRACE*, *CLONE_SIGHAND*, *CLONE_SYSVSEM*, *CLONE_THREAD*, *CLONE_UNTRACED*, *CLONE_VM*, *CLONE_INVERTED*, *NAME_CHANGED*, *CLOSED*, *CLONE_NEWUSER*, *CLONE_CHILD_CLEARTID*, *CLONE_CHILD_SETTID*, *CLONE_SETTLS*, *CLONE_STOPPED*, *CLONE_VFORK*, *CLONE_NEWCGROUP*, UINT32 **uid**, UINT32 **gid**, PID **vtid**, PID **vpid**, UINT64 **pidns_init_start_ts**
Yes | `>` | `fork` |
Yes | `<` | `fork` | PID **res**, CHARBUF **exe**, BYTEBUF **args**, PID **tid**, PID **pid**, PID **ptid**, CHARBUF **cwd**, INT64 **fdlimit**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**, CHARBUF **comm**, BYTEBUF **cgroups**, FLAGS32 **flags**: *CLONE_FILES*, *CLONE_FS*, *CLONE_IO*, *CLONE_NEWIPC*, *CLONE_NEWNET*, *CLONE_NEWNS*, *CLONE_NEWPID*, *CLONE_NEWUTS*, *CLONE_PARENT*, *CLONE_PARENT_SETTID*, *CLONE_PTRACE*, *CLONE_SIGHAND*, *CLONE_SYSVSEM*, *CLONE_THREAD*, *CLONE_UNTRACED*, *CLONE_VM*, *CLONE_INVERTED*, *NAME_CHANGED*, *CLOSED*, *CLONE_NEWUSER*, *CLONE_CHILD_CLEARTID*, *CLONE_CHILD_SETTID*, *CLONE_SETTLS*, *CLONE_STOPPED*, *CLONE_VFORK*, *CLONE_NEWCGROUP*, UINT32 **uid**, UINT32 **gid**, PID **vtid**, PID **vpid**, UINT64 **pidns_init_start_ts**
Yes | `>` | `vfork` |
Yes | `<` | `vfork` | PID **res**, CHARBUF **exe**, BYTEBUF **args**, PID **tid**, PID **pid**, PID **ptid**, CHARBUF **cwd**, INT64 **fdlimit**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**, CHARBUF **comm**, BYTEBUF **cgroups**, FLAGS32 **flags**: *CLONE_FILES*, *CLONE_FS*, *CLONE_IO*, *CLONE_NEWIPC*, *CLONE_NEWNET*, *CLONE_NEWNS*, *CLONE_NEWPID*, *CLONE_NEWUTS*, *CLONE_PARENT*, *CLONE_PARENT_SETTID*, *CLONE_PTRACE*, *CLONE_SIGHAND*, *CLONE_SYSVSEM*, *CLONE_THREAD*, *CLONE_UNTRACED*, *CLONE_VM*, *CLONE_INVERTED*, *NAME_CHANGED*, *CLOSED*, *CLONE_NEWUSER*, *CLONE_CHILD_CLEARTID*, *CLONE_CHILD_SETTID*, *CLONE_SETTLS*, *CLONE_STOPPED*, *CLONE_VFORK*, *CLONE_NEWCGROUP*, UINT32 **uid**, UINT32 **gid**, PID **vtid**, PID **vpid**, UINT64 **pidns_init_start_ts**
Yes | `>` | `getdents` | FD **fd**
Yes | `<` | `getdents` | ERRNO **res**
Yes | `>` | `getdents64` | FD **fd**
Yes | `<` | `getdents64` | ERRNO **res**
Yes | `>` | `setns` | FD **fd**, FLAGS32 **nstype**: *CLONE_FILES*, *CLONE_FS*, *CLONE_IO*, *CLONE_NEWIPC*, *CLONE_NEWNET*, *CLONE_NEWNS*, *CLONE_NEWPID*, *CLONE_NEWUTS*, *CLONE_PARENT*, *CLONE_PARENT_SETTID*, *CLONE_PTRACE*, *CLONE_SIGHAND*, *CLONE_SYSVSEM*, *CLONE_THREAD*, *CLONE_UNTRACED*, *CLONE_VM*, *CLONE_INVERTED*, *NAME_CHANGED*, *CLOSED*, *CLONE_NEWUSER*, *CLONE_CHILD_CLEARTID*, *CLONE_CHILD_SETTID*, *CLONE_SETTLS*, *CLONE_STOPPED*, *CLONE_VFORK*, *CLONE_NEWCGROUP*
Yes | `<` | `setns` | ERRNO **res**
Yes | `>` | `flock` | FD **fd**, FLAGS32 **operation**: *LOCK_SH*, *LOCK_EX*, *LOCK_NB*, *LOCK_UN*, *LOCK_NONE*
Yes | `<` | `flock` | ERRNO **res**
Yes | `>` | `accept` |
Yes | `<` | `accept` | FD **fd**, SOCKTUPLE **tuple**, UINT8 **queuepct**, UINT32 **queuelen**, UINT32 **queuemax**
Yes | `>` | `semop` | INT32 **semid**
Yes | `<` | `semop` | ERRNO **res**, UINT32 **nsops**, UINT16 **sem_num_0**, INT16 **sem_op_0**, FLAGS16 **sem_flg_0**: *IPC_NOWAIT*, *SEM_UNDO*, UINT16 **sem_num_1**, INT16 **sem_op_1**, FLAGS16 **sem_flg_1**: *IPC_NOWAIT*, *SEM_UNDO*
Yes | `>` | `semctl` | INT32 **semid**, INT32 **semnum**, FLAGS16 **cmd**: *IPC_STAT*, *IPC_SET*, *IPC_RMID*, *IPC_INFO*, *SEM_INFO*, *SEM_STAT*, *GETALL*, *GETNCNT*, *GETPID*, *GETVAL*, *GETZCNT*, *SETALL*, *SETVAL*, INT32 **val**
Yes | `<` | `semctl` | ERRNO **res**
Yes | `>` | `ppoll` | FDLIST **fds**, RELTIME **timeout**, SIGSET **sigmask**
Yes | `<` | `ppoll` | ERRNO **res**, FDLIST **fds**
Yes | `>` | `mount` | FLAGS32 **flags**: *RDONLY*, *NOSUID*, *NODEV*, *NOEXEC*, *SYNCHRONOUS*, *REMOUNT*, *MANDLOCK*, *DIRSYNC*, *NOATIME*, *NODIRATIME*, *BIND*, *MOVE*, *REC*, *SILENT*, *POSIXACL*, *UNBINDABLE*, *PRIVATE*, *SLAVE*, *SHARED*, *RELATIME*, *KERNMOUNT*, *I_VERSION*, *STRICTATIME*, *LAZYTIME*, *NOSEC*, *BORN*, *ACTIVE*, *NOUSER*
Yes | `<` | `mount` | ERRNO **res**, CHARBUF **dev**, FSPATH **dir**, CHARBUF **type**
Yes | `>` | `semget` | INT32 **key**, INT32 **nsems**, FLAGS32 **semflg**: *IPC_EXCL*, *IPC_CREAT*
Yes | `<` | `semget` | ERRNO **res**
Yes | `>` | `access` | FLAGS32 **mode**: *F_OK*, *R_OK*, *W_OK*, *X_OK*
Yes | `<` | `access` | ERRNO **res**, FSPATH **name**
Yes | `>` | `chroot` |
Yes | `<` | `chroot` | ERRNO **res**, FSPATH **path**
Yes | `>` | `setsid` |
Yes | `<` | `setsid` | PID **res**
Yes | `>` | `mkdir` | UINT32 **mode**
Yes | `<` | `mkdir` | ERRNO **res**, FSPATH **path**
Yes | `>` | `rmdir` |
Yes | `<` | `rmdir` | ERRNO **res**, FSPATH **path**
Yes | `>` | `unshare` | FLAGS32 **flags**: *CLONE_FILES*, *CLONE_FS*, *CLONE_IO*, *CLONE_NEWIPC*, *CLONE_NEWNET*, *CLONE_NEWNS*, *CLONE_NEWPID*, *CLONE_NEWUTS*, *CLONE_PARENT*, *CLONE_PARENT_SETTID*, *CLONE_PTRACE*, *CLONE_SIGHAND*, *CLONE_SYSVSEM*, *CLONE_THREAD*, *CLONE_UNTRACED*, *CLONE_VM*, *CLONE_INVERTED*, *NAME_CHANGED*, *CLOSED*, *CLONE_NEWUSER*, *CLONE_CHILD_CLEARTID*, *CLONE_CHILD_SETTID*, *CLONE_SETTLS*, *CLONE_STOPPED*, *CLONE_VFORK*, *CLONE_NEWCGROUP*
Yes | `<` | `unshare` | ERRNO **res**
Yes | `>` | `execve` | FSPATH **filename**
Yes | `<` | `execve` | ERRNO **res**, CHARBUF **exe**, BYTEBUF **args**, PID **tid**, PID **pid**, PID **ptid**, CHARBUF **cwd**, UINT64 **fdlimit**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**, CHARBUF **comm**, BYTEBUF **cgroups**, BYTEBUF **env**, UINT32 **tty**, PID **pgid**, UID **loginuid**, FLAGS32 **flags**: *EXE_WRITABLE*, *EXE_UPPER_LAYER*, *EXE_FROM_MEMFD*, UINT64 **cap_inheritable**, UINT64 **cap_permitted**, UINT64 **cap_effective**, UINT64 **exe_ino**, ABSTIME **exe_ino_ctime**, ABSTIME **exe_ino_mtime**, UID **uid**, FSPATH **trusted_exepath**
Yes | `>` | `setpgid` | PID **pid**, PID **pgid**
Yes | `<` | `setpgid` | PID **res**
Yes | `>` | `seccomp` | UINT64 **op**, UINT64 **flags**
Yes | `<` | `seccomp` | ERRNO **res**
Yes | `>` | `unlink` |
Yes | `<` | `unlink` | ERRNO **res**, FSPATH **path**
Yes | `>` | `unlinkat` |
Yes | `<` | `unlinkat` | ERRNO **res**, FD **dirfd**, FSRELPATH **name**, FLAGS32 **flags**: *AT_REMOVEDIR*
Yes | `>` | `mkdirat` |
Yes | `<` | `mkdirat` | ERRNO **res**, FD **dirfd**, FSRELPATH **path**, UINT32 **mode**
Yes | `>` | `openat` | FD **dirfd**, FSRELPATH **name**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, UINT32 **mode**
Yes | `<` | `openat` | FD **fd**, FD **dirfd**, FSRELPATH **name**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, UINT32 **mode**, UINT32 **dev**, UINT64 **ino**
Yes | `>` | `link` |
Yes | `<` | `link` | ERRNO **res**, FSPATH **oldpath**, FSPATH **newpath**
Yes | `>` | `linkat` |
Yes | `<` | `linkat` | ERRNO **res**, FD **olddir**, FSRELPATH **oldpath**, FD **newdir**, FSRELPATH **newpath**, FLAGS32 **flags**: *AT_SYMLINK_FOLLOW*, *AT_EMPTY_PATH*
Yes | `>` | `fchmodat` |
Yes | `<` | `fchmodat` | ERRNO **res**, FD **dirfd**, FSRELPATH **filename**, MODE **mode**
Yes | `>` | `chmod` |
Yes | `<` | `chmod` | ERRNO **res**, FSPATH **filename**, MODE **mode**
Yes | `>` | `fchmod` |
Yes | `<` | `fchmod` | ERRNO **res**, FD **fd**, MODE **mode**
Yes | `>` | `renameat2` |
Yes | `<` | `renameat2` | ERRNO **res**, FD **olddirfd**, FSRELPATH **oldpath**, FD **newdirfd**, FSRELPATH **newpath**, FLAGS32 **flags**: *RENAME_NOREPLACE*, *RENAME_EXCHANGE*, *RENAME_WHITEOUT*
Yes | `>` | `userfaultfd` |
Yes | `<` | `userfaultfd` | ERRNO **res**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*
Yes | `>` | `openat2` | FD **dirfd**, FSRELPATH **name**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, UINT32 **mode**, FLAGS32 **resolve**: *RESOLVE_BENEATH*, *RESOLVE_IN_ROOT*, *RESOLVE_NO_MAGICLINKS*, *RESOLVE_NO_SYMLINKS*, *RESOLVE_NO_XDEV*, *RESOLVE_CACHED*
Yes | `<` | `openat2` | FD **fd**, FD **dirfd**, FSRELPATH **name**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, UINT32 **mode**, FLAGS32 **resolve**: *RESOLVE_BENEATH*, *RESOLVE_IN_ROOT*, *RESOLVE_NO_MAGICLINKS*, *RESOLVE_NO_SYMLINKS*, *RESOLVE_NO_XDEV*, *RESOLVE_CACHED*, UINT32 **dev**, UINT64 **ino**
Yes | `>` | `mprotect` | UINT64 **addr**, UINT64 **length**, FLAGS32 **prot**: *PROT_READ*, *PROT_WRITE*, *PROT_EXEC*, *PROT_SEM*, *PROT_GROWSDOWN*, *PROT_GROWSUP*, *PROT_SAO*, *PROT_NONE*
Yes | `<` | `mprotect` | ERRNO **res**
Yes | `>` | `execveat` | FD **dirfd**, FSRELPATH **pathname**, FLAGS32 **flags**: *AT_EMPTY_PATH*, *AT_SYMLINK_NOFOLLOW*
Yes | `<` | `execveat` | ERRNO **res**, CHARBUF **exe**, BYTEBUF **args**, PID **tid**, PID **pid**, PID **ptid**, CHARBUF **cwd**, UINT64 **fdlimit**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**, CHARBUF **comm**, BYTEBUF **cgroups**, BYTEBUF **env**, UINT32 **tty**, PID **pgid**, UID **loginuid**, FLAGS32 **flags**: *EXE_WRITABLE*, *EXE_UPPER_LAYER*, *EXE_FROM_MEMFD*, UINT64 **cap_inheritable**, UINT64 **cap_permitted**, UINT64 **cap_effective**, UINT64 **exe_ino**, ABSTIME **exe_ino_ctime**, ABSTIME **exe_ino_mtime**, UID **uid**, FSPATH **trusted_exepath**
Yes | `>` | `copy_file_range` | FD **fdin**, UINT64 **offin**, UINT64 **len**
Yes | `<` | `copy_file_range` | ERRNO **res**, FD **fdout**, UINT64 **offout**
Yes | `>` | `clone3` |
Yes | `<` | `clone3` | PID **res**, CHARBUF **exe**, BYTEBUF **args**, PID **tid**, PID **pid**, PID **ptid**, CHARBUF **cwd**, INT64 **fdlimit**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**, CHARBUF **comm**, BYTEBUF **cgroups**, FLAGS32 **flags**: *CLONE_FILES*, *CLONE_FS*, *CLONE_IO*, *CLONE_NEWIPC*, *CLONE_NEWNET*, *CLONE_NEWNS*, *CLONE_NEWPID*, *CLONE_NEWUTS*, *CLONE_PARENT*, *CLONE_PARENT_SETTID*, *CLONE_PTRACE*, *CLONE_SIGHAND*, *CLONE_SYSVSEM*, *CLONE_THREAD*, *CLONE_UNTRACED*, *CLONE_VM*, *CLONE_INVERTED*, *NAME_CHANGED*, *CLOSED*, *CLONE_NEWUSER*, *CLONE_CHILD_CLEARTID*, *CLONE_CHILD_SETTID*, *CLONE_SETTLS*, *CLONE_STOPPED*, *CLONE_VFORK*, *CLONE_NEWCGROUP*, UINT32 **uid**, UINT32 **gid**, PID **vtid**, PID **vpid**, UINT64 **pidns_init_start_ts**
Yes | `>` | `open_by_handle_at` |
Yes | `<` | `open_by_handle_at` | FD **fd**, FD **mountfd**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*, FSPATH **path**, UINT32 **dev**, UINT64 **ino**
Yes | `>` | `io_uring_setup` |
Yes | `<` | `io_uring_setup` | ERRNO **res**, UINT32 **entries**, UINT32 **sq_entries**, UINT32 **cq_entries**, FLAGS32 **flags**: *IORING_SETUP_IOPOLL*, *IORING_SETUP_SQPOLL*, *IORING_SQ_NEED_WAKEUP*, *IORING_SETUP_SQ_AFF*, *IORING_SETUP_CQSIZE*, *IORING_SETUP_CLAMP*, *IORING_SETUP_ATTACH_RW*, *IORING_SETUP_R_DISABLED*, UINT32 **sq_thread_cpu**, UINT32 **sq_thread_idle**, FLAGS32 **features**: *IORING_FEAT_SINGLE_MMAP*, *IORING_FEAT_NODROP*, *IORING_FEAT_SUBMIT_STABLE*, *IORING_FEAT_RW_CUR_POS*, *IORING_FEAT_CUR_PERSONALITY*, *IORING_FEAT_FAST_POLL*, *IORING_FEAT_POLL_32BITS*, *IORING_FEAT_SQPOLL_NONFIXED*, *IORING_FEAT_ENTER_EXT_ARG*, *IORING_FEAT_NATIVE_WORKERS*, *IORING_FEAT_RSRC_TAGS*
Yes | `>` | `io_uring_enter` |
Yes | `<` | `io_uring_enter` | ERRNO **res**, FD **fd**, UINT32 **to_submit**, UINT32 **min_complete**, FLAGS32 **flags**: *IORING_ENTER_GETEVENTS*, *IORING_ENTER_SQ_WAKEUP*, *IORING_ENTER_SQ_WAIT*, *IORING_ENTER_EXT_ARG*, SIGSET **sig**
Yes | `>` | `io_uring_register` |
Yes | `<` | `io_uring_register` | ERRNO **res**, FD **fd**, ENUMFLAGS16 **opcode**: *IORING_REGISTER_BUFFERS*, *IORING_UNREGISTER_BUFFERS*, *IORING_REGISTER_FILES*, *IORING_UNREGISTER_FILES*, *IORING_REGISTER_EVENTFD*, *IORING_UNREGISTER_EVENTFD*, *IORING_REGISTER_FILES_UPDATE*, *IORING_REGISTER_EVENTFD_ASYNC*, *IORING_REGISTER_PROBE*, *IORING_REGISTER_PERSONALITY*, *IORING_UNREGISTER_PERSONALITY*, *IORING_REGISTER_RESTRICTIONS*, *IORING_REGISTER_ENABLE_RINGS*, *IORING_REGISTER_FILES2*, *IORING_REGISTER_FILES_UPDATE2*, *IORING_REGISTER_BUFFERS2*, *IORING_REGISTER_BUFFERS_UPDATE*, *IORING_REGISTER_IOWQ_AFF*, *IORING_UNREGISTER_IOWQ_AFF*, *IORING_REGISTER_IOWQ_MAX_WORKERS*, *IORING_REGISTER_RING_FDS*, *IORING_UNREGISTER_RING_FDS*, UINT64 **arg**, UINT32 **nr_args**
Yes | `>` | `mlock` |
Yes | `<` | `mlock` | ERRNO **res**, UINT64 **addr**, UINT64 **len**
Yes | `>` | `munlock` |
Yes | `<` | `munlock` | ERRNO **res**, UINT64 **addr**, UINT64 **len**
Yes | `>` | `mlockall` |
Yes | `<` | `mlockall` | ERRNO **res**, FLAGS32 **flags**: *MCL_CURRENT*, *MCL_FUTURE*, *MCL_ONFAULT*
Yes | `>` | `munlockall` |
Yes | `<` | `munlockall` | ERRNO **res**
Yes | `>` | `capset` |
Yes | `<` | `capset` | ERRNO **res**, UINT64 **cap_inheritable**, UINT64 **cap_permitted**, UINT64 **cap_effective**
Yes | `>` | `dup2` | FD **fd**
Yes | `<` | `dup2` | FD **res**, FD **oldfd**, FD **newfd**
Yes | `>` | `dup3` | FD **fd**
Yes | `<` | `dup3` | FD **res**, FD **oldfd**, FD **newfd**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*
Yes | `>` | `dup` | FD **fd**
Yes | `<` | `dup` | FD **res**, FD **oldfd**
Yes | `>` | `bpf` | INT64 **cmd**
Yes | `<` | `bpf` | FD **fd**, ENUMFLAGS32 **cmd**: *BPF_MAP_CREATE*, *BPF_MAP_LOOKUP_ELEM*, *BPF_MAP_UPDATE_ELEM*, *BPF_MAP_DELETE_ELEM*, *BPF_MAP_GET_NEXT_KEY*, *BPF_PROG_LOAD*, *BPF_OBJ_PIN*, *BPF_OBJ_GET*, *BPF_PROG_ATTACH*, *BPF_PROG_DETACH*, *BPF_PROG_TEST_RUN*, *BPF_PROG_RUN*, *BPF_PROG_GET_NEXT_ID*, *BPF_MAP_GET_NEXT_ID*, *BPF_PROG_GET_FD_BY_ID*, *BPF_MAP_GET_FD_BY_ID*, *BPF_OBJ_GET_INFO_BY_FD*, *BPF_PROG_QUERY*, *BPF_RAW_TRACEPOINT_OPEN*, *BPF_BTF_LOAD*, *BPF_BTF_GET_FD_BY_ID*, *BPF_TASK_FD_QUERY*, *BPF_MAP_LOOKUP_AND_DELETE_ELEM*, *BPF_MAP_FREEZE*, *BPF_BTF_GET_NEXT_ID*, *BPF_MAP_LOOKUP_BATCH*, *BPF_MAP_LOOKUP_AND_DELETE_BATCH*, *BPF_MAP_UPDATE_BATCH*, *BPF_MAP_DELETE_BATCH*, *BPF_LINK_CREATE*, *BPF_LINK_UPDATE*, *BPF_LINK_GET_FD_BY_ID*, *BPF_LINK_GET_NEXT_ID*, *BPF_ENABLE_STATS*, *BPF_ITER_CREATE*, *BPF_LINK_DETACH*, *BPF_PROG_BIND_MAP*
Yes | `>` | `mlock2` |
Yes | `<` | `mlock2` | ERRNO **res**, UINT64 **addr**, UINT64 **len**, FLAGS32 **flags**: *MLOCK_ONFAULT*
Yes | `>` | `fsconfig` |
Yes | `<` | `fsconfig` | ERRNO **res**, FD **fd**, ENUMFLAGS32 **cmd**: *FSCONFIG_SET_FLAG*, *FSCONFIG_SET_STRING*, *FSCONFIG_SET_BINARY*, *FSCONFIG_SET_PATH*, *FSCONFIG_SET_PATH_EMPTY*, *FSCONFIG_SET_FD*, *FSCONFIG_CMD_CREATE*, *FSCONFIG_CMD_RECONFIGURE*, CHARBUF **key**, BYTEBUF **value_bytebuf**, CHARBUF **value_charbuf**, INT32 **aux**
Yes | `>` | `epoll_create` | INT32 **size**
Yes | `<` | `epoll_create` | ERRNO **res**
Yes | `>` | `epoll_create1` | FLAGS32 **flags**: *EPOLL_CLOEXEC*
Yes | `<` | `epoll_create1` | ERRNO **res**
Yes | `>` | `chown` |
Yes | `<` | `chown` | ERRNO **res**, FSPATH **path**, UINT32 **uid**, UINT32 **gid**
Yes | `>` | `lchown` |
Yes | `<` | `lchown` | ERRNO **res**, FSPATH **path**, UINT32 **uid**, UINT32 **gid**
Yes | `>` | `fchown` |
Yes | `<` | `fchown` | ERRNO **res**, FD **fd**, UINT32 **uid**, UINT32 **gid**
Yes | `>` | `fchownat` |
Yes | `<` | `fchownat` | ERRNO **res**, FD **dirfd**, FSRELPATH **pathname**, UINT32 **uid**, UINT32 **gid**, FLAGS32 **flags**: *AT_SYMLINK_NOFOLLOW*, *AT_EMPTY_PATH*
Yes | `>` | `umount` |
Yes | `<` | `umount` | ERRNO **res**, FSPATH **name**
Yes | `>` | `accept4` | INT32 **flags**
Yes | `<` | `accept4` | FD **fd**, SOCKTUPLE **tuple**, UINT8 **queuepct**, UINT32 **queuelen**, UINT32 **queuemax**
Yes | `>` | `umount2` | FLAGS32 **flags**: *FORCE*, *DETACH*, *EXPIRE*, *NOFOLLOW*
Yes | `<` | `umount2` | ERRNO **res**, FSPATH **name**
Yes | `>` | `pipe2` |
Yes | `<` | `pipe2` | ERRNO **res**, FD **fd1**, FD **fd2**, UINT64 **ino**, FLAGS32 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*
Yes | `>` | `inotify_init1` |
Yes | `<` | `inotify_init1` | FD **res**, FLAGS16 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*
Yes | `>` | `eventfd2` | UINT64 **initval**
Yes | `<` | `eventfd2` | FD **res**, FLAGS16 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*
Yes | `>` | `signalfd4` | FD **fd**, UINT32 **mask**
Yes | `<` | `signalfd4` | FD **res**, FLAGS16 **flags**: *O_LARGEFILE*, *O_DIRECTORY*, *O_DIRECT*, *O_TRUNC*, *O_SYNC*, *O_NONBLOCK*, *O_EXCL*, *O_DSYNC*, *O_APPEND*, *O_CREAT*, *O_RDWR*, *O_WRONLY*, *O_RDONLY*, *O_CLOEXEC*, *O_NONE*, *O_TMPFILE*, *O_F_CREATED*
Yes | `>` | `prctl` |
Yes | `<` | `prctl` | ERRNO **res**, ENUMFLAGS32 **option**: *PR_GET_DUMPABLE*, *PR_SET_DUMPABLE*, *PR_GET_KEEPCAPS*, *PR_SET_KEEPCAPS*, *PR_SET_NAME*, *PR_GET_NAME*, *PR_GET_SECCOMP*, *PR_SET_SECCOMP*, *PR_CAPBSET_READ*, *PR_CAPBSET_DROP*, *PR_GET_SECUREBITS*, *PR_SET_SECUREBITS*, *PR_MCE_KILL*, *PR_MCE_KILL*, *PR_SET_MM*, *PR_SET_CHILD_SUBREAPER*, *PR_GET_CHILD_SUBREAPER*, *PR_SET_NO_NEW_PRIVS*, *PR_GET_NO_NEW_PRIVS*, *PR_GET_TID_ADDRESS*, *PR_SET_THP_DISABLE*, *PR_GET_THP_DISABLE*, *PR_CAP_AMBIENT*, CHARBUF **arg2_str**, INT64 **arg2_int**
Yes | `>` | `memfd_create` |
Yes | `<` | `memfd_create` | FD **fd**, CHARBUF **name**, FLAGS32 **flags**: *MFD_CLOEXEC*, *MFD_ALLOW_SEALING*, *MFD_HUGETLB*
Yes | `>` | `pidfd_getfd` |
Yes | `<` | `pidfd_getfd` | FD **fd**, FD **pid_fd**, FD **target_fd**, UINT32 **flags**
Yes | `>` | `pidfd_open` |
Yes | `<` | `pidfd_open` | FD **fd**, PID **pid**, FLAGS32 **flags**: *PIDFD_NONBLOCK*
Yes | `>` | `init_module` |
Yes | `<` | `init_module` | ERRNO **res**, BYTEBUF **img**, UINT64 **length**, CHARBUF **uargs**
Yes | `>` | `finit_module` |
Yes | `<` | `finit_module` | ERRNO **res**, FD **fd**, CHARBUF **uargs**, FLAGS32 **flags**: *MODULE_INIT_IGNORE_MODVERSIONS*, *MODULE_INIT_IGNORE_VERMAGIC*, *MODULE_INIT_COMPRESSED_FILE*
Yes | `>` | `mknod` |
Yes | `<` | `mknod` | ERRNO **res**, FSPATH **path**, MODE **mode**, UINT32 **dev**
Yes | `>` | `mknodat` |
Yes | `<` | `mknodat` | ERRNO **res**, FD **dirfd**, FSRELPATH **path**, MODE **mode**, UINT32 **dev**
Yes | `>` | `newfstatat` |
Yes | `<` | `newfstatat` | ERRNO **res**, FD **dirfd**, FSRELPATH **path**, FLAGS32 **flags**: *AT_EMPTY_PATH*, *AT_NO_AUTOMOUNT*, *AT_SYMLINK_NOFOLLOW*
Yes | `>` | `process_vm_readv` |
Yes | `<` | `process_vm_readv` | INT64 **res**, PID **pid**, BYTEBUF **data**
Yes | `>` | `process_vm_writev` |
Yes | `<` | `process_vm_writev` | INT64 **res**, PID **pid**, BYTEBUF **data**
Yes | `>` | `delete_module` |
Yes | `<` | `delete_module` | ERRNO **res**, CHARBUF **name**, FLAGS32 **flags**: *O_NONBLOCK*, *O_TRUNC*
Yes | `>` | `lsm_get_self_attr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lsm_get_self_attr` | SYSCALLID **ID**
Yes | `>` | `listmount` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `listmount` | SYSCALLID **ID**
Yes | `>` | `vm86` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `vm86` | SYSCALLID **ID**
Yes | `>` | `pciconfig_read` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pciconfig_read` | SYSCALLID **ID**
Yes | `>` | `rtas` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rtas` | SYSCALLID **ID**
Yes | `>` | `pciconfig_write` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pciconfig_write` | SYSCALLID **ID**
Yes | `>` | `swapcontext` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `swapcontext` | SYSCALLID **ID**
Yes | `>` | `spu_run` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `spu_run` | SYSCALLID **ID**
Yes | `>` | `oldfstat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `oldfstat` | SYSCALLID **ID**
Yes | `>` | `sync_file_range2` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sync_file_range2` | SYSCALLID **ID**
Yes | `>` | `spu_create` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `spu_create` | SYSCALLID **ID**
Yes | `>` | `oldlstat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `oldlstat` | SYSCALLID **ID**
Yes | `>` | `oldstat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `oldstat` | SYSCALLID **ID**
Yes | `>` | `riscv_flush_icache` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `riscv_flush_icache` | SYSCALLID **ID**
Yes | `>` | `cachestat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `cachestat` | SYSCALLID **ID**
Yes | `>` | `sigreturn` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sigreturn` | SYSCALLID **ID**
Yes | `>` | `s390_runtime_instr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `s390_runtime_instr` | SYSCALLID **ID**
Yes | `>` | `idle` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `idle` | SYSCALLID **ID**
Yes | `>` | `s390_sthyi` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `s390_sthyi` | SYSCALLID **ID**
Yes | `>` | `sigaction` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sigaction` | SYSCALLID **ID**
Yes | `>` | `s390_pci_mmio_read` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `s390_pci_mmio_read` | SYSCALLID **ID**
Yes | `>` | `timerfd` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timerfd` | SYSCALLID **ID**
Yes | `>` | `fanotify_mark` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fanotify_mark` | SYSCALLID **ID**
Yes | `>` | `close_range` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `close_range` | SYSCALLID **ID**
Yes | `>` | `process_madvise` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `process_madvise` | SYSCALLID **ID**
Yes | `>` | `sync_file_range` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sync_file_range` | SYSCALLID **ID**
Yes | `>` | `get_mempolicy` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `get_mempolicy` | SYSCALLID **ID**
Yes | `>` | `query_module` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `query_module` | SYSCALLID **ID**
Yes | `>` | `_sysctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `_sysctl` | SYSCALLID **ID**
Yes | `>` | `nfsservctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `nfsservctl` | SYSCALLID **ID**
Yes | `>` | `futex_waitv` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `futex_waitv` | SYSCALLID **ID**
Yes | `>` | `readahead` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `readahead` | SYSCALLID **ID**
Yes | `>` | `set_mempolicy_home_node` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `set_mempolicy_home_node` | SYSCALLID **ID**
Yes | `>` | `tee` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `tee` | SYSCALLID **ID**
Yes | `>` | `vmsplice` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `vmsplice` | SYSCALLID **ID**
Yes | `>` | `msgget` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `msgget` | SYSCALLID **ID**
Yes | `>` | `io_getevents` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `io_getevents` | SYSCALLID **ID**
Yes | `>` | `timerfd_settime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timerfd_settime` | SYSCALLID **ID**
Yes | `>` | `rseq` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rseq` | SYSCALLID **ID**
Yes | `>` | `set_thread_area` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `set_thread_area` | SYSCALLID **ID**
Yes | `>` | `fremovexattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fremovexattr` | SYSCALLID **ID**
Yes | `>` | `removexattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `removexattr` | SYSCALLID **ID**
Yes | `>` | `s390_pci_mmio_write` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `s390_pci_mmio_write` | SYSCALLID **ID**
Yes | `>` | `flistxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `flistxattr` | SYSCALLID **ID**
Yes | `>` | `sched_get_priority_min` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_get_priority_min` | SYSCALLID **ID**
Yes | `>` | `remap_file_pages` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `remap_file_pages` | SYSCALLID **ID**
Yes | `>` | `mbind` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mbind` | SYSCALLID **ID**
Yes | `>` | `getxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getxattr` | SYSCALLID **ID**
Yes | `>` | `gettid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `gettid` | SYSCALLID **ID**
Yes | `>` | `shmat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `shmat` | SYSCALLID **ID**
Yes | `>` | `setfsgid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setfsgid` | SYSCALLID **ID**
Yes | `>` | `get_kernel_syms` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `get_kernel_syms` | SYSCALLID **ID**
Yes | `>` | `setgroups` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setgroups` | SYSCALLID **ID**
Yes | `>` | `getpmsg` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getpmsg` | SYSCALLID **ID**
Yes | `>` | `rt_sigaction` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigaction` | SYSCALLID **ID**
Yes | `>` | `getgroups` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getgroups` | SYSCALLID **ID**
Yes | `>` | `io_cancel` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `io_cancel` | SYSCALLID **ID**
Yes | `>` | `setreuid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setreuid` | SYSCALLID **ID**
Yes | `>` | `capget` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `capget` | SYSCALLID **ID**
Yes | `>` | `madvise` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `madvise` | SYSCALLID **ID**
Yes | `>` | `lsm_list_modules` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lsm_list_modules` | SYSCALLID **ID**
Yes | `>` | `setdomainname` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setdomainname` | SYSCALLID **ID**
Yes | `>` | `rt_sigsuspend` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigsuspend` | SYSCALLID **ID**
Yes | `>` | `pciconfig_iobase` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pciconfig_iobase` | SYSCALLID **ID**
Yes | `>` | `rt_sigqueueinfo` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigqueueinfo` | SYSCALLID **ID**
Yes | `>` | `preadv2` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `preadv2` | SYSCALLID **ID**
Yes | `>` | `io_destroy` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `io_destroy` | SYSCALLID **ID**
Yes | `>` | `name_to_handle_at` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `name_to_handle_at` | SYSCALLID **ID**
Yes | `>` | `setxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setxattr` | SYSCALLID **ID**
Yes | `>` | `faccessat2` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `faccessat2` | SYSCALLID **ID**
Yes | `>` | `rt_sigtimedwait` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigtimedwait` | SYSCALLID **ID**
Yes | `>` | `timer_gettime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timer_gettime` | SYSCALLID **ID**
Yes | `>` | `switch_endian` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `switch_endian` | SYSCALLID **ID**
Yes | `>` | `s390_guarded_storage` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `s390_guarded_storage` | SYSCALLID **ID**
Yes | `>` | `timer_create` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timer_create` | SYSCALLID **ID**
Yes | `>` | `swapon` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `swapon` | SYSCALLID **ID**
Yes | `>` | `rt_sigprocmask` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigprocmask` | SYSCALLID **ID**
Yes | `>` | `faccessat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `faccessat` | SYSCALLID **ID**
Yes | `>` | `lremovexattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lremovexattr` | SYSCALLID **ID**
Yes | `>` | `multiplexer` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `multiplexer` | SYSCALLID **ID**
Yes | `>` | `sched_setscheduler` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_setscheduler` | SYSCALLID **ID**
Yes | `>` | `reboot` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `reboot` | SYSCALLID **ID**
Yes | `>` | `getsid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getsid` | SYSCALLID **ID**
Yes | `>` | `futex_wake` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `futex_wake` | SYSCALLID **ID**
Yes | `>` | `settimeofday` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `settimeofday` | SYSCALLID **ID**
Yes | `>` | `getrusage` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getrusage` | SYSCALLID **ID**
Yes | `>` | `setitimer` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setitimer` | SYSCALLID **ID**
Yes | `>` | `lsm_set_self_attr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lsm_set_self_attr` | SYSCALLID **ID**
Yes | `>` | `setregid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setregid` | SYSCALLID **ID**
Yes | `>` | `vhangup` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `vhangup` | SYSCALLID **ID**
Yes | `>` | `get_thread_area` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `get_thread_area` | SYSCALLID **ID**
Yes | `>` | `alarm` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `alarm` | SYSCALLID **ID**
Yes | `>` | `wait4` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `wait4` | SYSCALLID **ID**
Yes | `>` | `sched_setattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_setattr` | SYSCALLID **ID**
Yes | `>` | `utimes` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `utimes` | SYSCALLID **ID**
Yes | `>` | `times` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `times` | SYSCALLID **ID**
Yes | `>` | `truncate` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `truncate` | SYSCALLID **ID**
Yes | `>` | `sched_getparam` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_getparam` | SYSCALLID **ID**
Yes | `>` | `sched_getscheduler` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_getscheduler` | SYSCALLID **ID**
Yes | `>` | `umask` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `umask` | SYSCALLID **ID**
Yes | `>` | `rt_sigpending` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigpending` | SYSCALLID **ID**
Yes | `>` | `pkey_alloc` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pkey_alloc` | SYSCALLID **ID**
Yes | `>` | `io_pgetevents` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `io_pgetevents` | SYSCALLID **ID**
Yes | `>` | `set_tid_address` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `set_tid_address` | SYSCALLID **ID**
Yes | `>` | `uselib` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `uselib` | SYSCALLID **ID**
Yes | `>` | `sigsuspend` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sigsuspend` | SYSCALLID **ID**
Yes | `>` | `setfsuid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setfsuid` | SYSCALLID **ID**
Yes | `>` | `readdir` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `readdir` | SYSCALLID **ID**
Yes | `>` | `clock_gettime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `clock_gettime` | SYSCALLID **ID**
Yes | `>` | `gettimeofday` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `gettimeofday` | SYSCALLID **ID**
Yes | `>` | `restart_syscall` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `restart_syscall` | SYSCALLID **ID**
Yes | `>` | `mq_open` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mq_open` | SYSCALLID **ID**
Yes | `>` | `lsetxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lsetxattr` | SYSCALLID **ID**
Yes | `>` | `sysinfo` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sysinfo` | SYSCALLID **ID**
Yes | `>` | `mremap` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mremap` | SYSCALLID **ID**
Yes | `>` | `epoll_pwait` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `epoll_pwait` | SYSCALLID **ID**
Yes | `>` | `getpriority` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getpriority` | SYSCALLID **ID**
Yes | `>` | `adjtimex` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `adjtimex` | SYSCALLID **ID**
Yes | `>` | `fdatasync` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fdatasync` | SYSCALLID **ID**
Yes | `>` | `fstatfs64` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fstatfs64` | SYSCALLID **ID**
Yes | `>` | `sched_getaffinity` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_getaffinity` | SYSCALLID **ID**
Yes | `>` | `pause` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pause` | SYSCALLID **ID**
Yes | `>` | `statfs` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `statfs` | SYSCALLID **ID**
Yes | `>` | `riscv_hwprobe` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `riscv_hwprobe` | SYSCALLID **ID**
Yes | `>` | `epoll_ctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `epoll_ctl` | SYSCALLID **ID**
Yes | `>` | `exit_group` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `exit_group` | SYSCALLID **ID**
Yes | `>` | `mq_timedsend` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mq_timedsend` | SYSCALLID **ID**
Yes | `>` | `getppid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getppid` | SYSCALLID **ID**
Yes | `>` | `getpid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getpid` | SYSCALLID **ID**
Yes | `>` | `lgetxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lgetxattr` | SYSCALLID **ID**
Yes | `>` | `process_mrelease` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `process_mrelease` | SYSCALLID **ID**
Yes | `>` | `kexec_load` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `kexec_load` | SYSCALLID **ID**
Yes | `>` | `ioprio_get` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ioprio_get` | SYSCALLID **ID**
Yes | `>` | `acct` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `acct` | SYSCALLID **ID**
Yes | `>` | `epoll_pwait2` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `epoll_pwait2` | SYSCALLID **ID**
Yes | `>` | `getpgrp` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getpgrp` | SYSCALLID **ID**
Yes | `>` | `sync` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sync` | SYSCALLID **ID**
Yes | `>` | `readlink` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `readlink` | SYSCALLID **ID**
Yes | `>` | `listxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `listxattr` | SYSCALLID **ID**
Yes | `>` | `sigprocmask` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sigprocmask` | SYSCALLID **ID**
Yes | `>` | `getpgid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getpgid` | SYSCALLID **ID**
Yes | `>` | `syslog` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `syslog` | SYSCALLID **ID**
Yes | `>` | `fstatfs` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fstatfs` | SYSCALLID **ID**
Yes | `>` | `ftruncate` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ftruncate` | SYSCALLID **ID**
Yes | `>` | `uname` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `uname` | SYSCALLID **ID**
Yes | `>` | `getitimer` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getitimer` | SYSCALLID **ID**
Yes | `>` | `exit` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `exit` | SYSCALLID **ID**
Yes | `>` | `swapoff` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `swapoff` | SYSCALLID **ID**
Yes | `>` | `fsetxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fsetxattr` | SYSCALLID **ID**
Yes | `>` | `utime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `utime` | SYSCALLID **ID**
Yes | `>` | `pivot_root` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pivot_root` | SYSCALLID **ID**
Yes | `>` | `ustat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ustat` | SYSCALLID **ID**
Yes | `>` | `mq_getsetattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mq_getsetattr` | SYSCALLID **ID**
Yes | `>` | `statmount` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `statmount` | SYSCALLID **ID**
Yes | `>` | `setpriority` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `setpriority` | SYSCALLID **ID**
Yes | `>` | `oldolduname` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `oldolduname` | SYSCALLID **ID**
Yes | `>` | `fsync` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fsync` | SYSCALLID **ID**
Yes | `>` | `pidfd_send_signal` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pidfd_send_signal` | SYSCALLID **ID**
Yes | `>` | `sched_yield` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_yield` | SYSCALLID **ID**
Yes | `>` | `quotactl_fd` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `quotactl_fd` | SYSCALLID **ID**
Yes | `>` | `io_setup` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `io_setup` | SYSCALLID **ID**
Yes | `>` | `llistxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `llistxattr` | SYSCALLID **ID**
Yes | `>` | `mincore` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mincore` | SYSCALLID **ID**
Yes | `>` | `sched_setparam` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_setparam` | SYSCALLID **ID**
Yes | `>` | `time` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `time` | SYSCALLID **ID**
Yes | `>` | `msync` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `msync` | SYSCALLID **ID**
Yes | `>` | `keyctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `keyctl` | SYSCALLID **ID**
Yes | `>` | `personality` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `personality` | SYSCALLID **ID**
Yes | `>` | `migrate_pages` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `migrate_pages` | SYSCALLID **ID**
Yes | `>` | `io_submit` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `io_submit` | SYSCALLID **ID**
Yes | `>` | `timer_settime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timer_settime` | SYSCALLID **ID**
Yes | `>` | `clock_nanosleep` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `clock_nanosleep` | SYSCALLID **ID**
Yes | `>` | `ioprio_set` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ioprio_set` | SYSCALLID **ID**
Yes | `>` | `inotify_add_watch` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `inotify_add_watch` | SYSCALLID **ID**
Yes | `>` | `fchmodat2` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fchmodat2` | SYSCALLID **ID**
Yes | `>` | `kexec_file_load` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `kexec_file_load` | SYSCALLID **ID**
Yes | `>` | `subpage_prot` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `subpage_prot` | SYSCALLID **ID**
Yes | `>` | `timer_getoverrun` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timer_getoverrun` | SYSCALLID **ID**
Yes | `>` | `timer_delete` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timer_delete` | SYSCALLID **ID**
Yes | `>` | `clock_getres` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `clock_getres` | SYSCALLID **ID**
Yes | `>` | `futex_wait` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `futex_wait` | SYSCALLID **ID**
Yes | `>` | `semtimedop` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `semtimedop` | SYSCALLID **ID**
Yes | `>` | `mq_unlink` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mq_unlink` | SYSCALLID **ID**
Yes | `>` | `mq_timedreceive` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mq_timedreceive` | SYSCALLID **ID**
Yes | `>` | `fallocate` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fallocate` | SYSCALLID **ID**
Yes | `>` | `waitid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `waitid` | SYSCALLID **ID**
Yes | `>` | `add_key` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `add_key` | SYSCALLID **ID**
Yes | `>` | `request_key` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `request_key` | SYSCALLID **ID**
Yes | `>` | `utimensat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `utimensat` | SYSCALLID **ID**
Yes | `>` | `inotify_rm_watch` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `inotify_rm_watch` | SYSCALLID **ID**
Yes | `>` | `futimesat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `futimesat` | SYSCALLID **ID**
Yes | `>` | `msgctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `msgctl` | SYSCALLID **ID**
Yes | `>` | `sgetmask` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sgetmask` | SYSCALLID **ID**
Yes | `>` | `sysfs` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sysfs` | SYSCALLID **ID**
Yes | `>` | `rt_sigreturn` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_sigreturn` | SYSCALLID **ID**
Yes | `>` | `readlinkat` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `readlinkat` | SYSCALLID **ID**
Yes | `>` | `pselect6` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pselect6` | SYSCALLID **ID**
Yes | `>` | `set_robust_list` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `set_robust_list` | SYSCALLID **ID**
Yes | `>` | `get_robust_list` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `get_robust_list` | SYSCALLID **ID**
Yes | `>` | `getcpu` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getcpu` | SYSCALLID **ID**
Yes | `>` | `fstatat64` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fstatat64` | SYSCALLID **ID**
Yes | `>` | `fgetxattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fgetxattr` | SYSCALLID **ID**
Yes | `>` | `perf_event_open` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `perf_event_open` | SYSCALLID **ID**
Yes | `>` | `rt_tgsigqueueinfo` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `rt_tgsigqueueinfo` | SYSCALLID **ID**
Yes | `>` | `mount_setattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mount_setattr` | SYSCALLID **ID**
Yes | `>` | `clock_settime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `clock_settime` | SYSCALLID **ID**
Yes | `>` | `epoll_ctl_old` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `epoll_ctl_old` | SYSCALLID **ID**
Yes | `>` | `clock_adjtime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `clock_adjtime` | SYSCALLID **ID**
Yes | `>` | `landlock_create_ruleset` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `landlock_create_ruleset` | SYSCALLID **ID**
Yes | `>` | `syncfs` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `syncfs` | SYSCALLID **ID**
Yes | `>` | `msgsnd` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `msgsnd` | SYSCALLID **ID**
Yes | `>` | `statfs64` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `statfs64` | SYSCALLID **ID**
Yes | `>` | `iopl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `iopl` | SYSCALLID **ID**
Yes | `>` | `msgrcv` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `msgrcv` | SYSCALLID **ID**
Yes | `>` | `shmdt` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `shmdt` | SYSCALLID **ID**
Yes | `>` | `timerfd_gettime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `timerfd_gettime` | SYSCALLID **ID**
Yes | `>` | `shmget` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `shmget` | SYSCALLID **ID**
Yes | `>` | `sethostname` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sethostname` | SYSCALLID **ID**
Yes | `>` | `mq_notify` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `mq_notify` | SYSCALLID **ID**
Yes | `>` | `shmctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `shmctl` | SYSCALLID **ID**
Yes | `>` | `move_pages` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `move_pages` | SYSCALLID **ID**
Yes | `>` | `fsmount` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fsmount` | SYSCALLID **ID**
Yes | `>` | `bdflush` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `bdflush` | SYSCALLID **ID**
Yes | `>` | `fanotify_init` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fanotify_init` | SYSCALLID **ID**
Yes | `>` | `ipc` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ipc` | SYSCALLID **ID**
Yes | `>` | `futex_requeue` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `futex_requeue` | SYSCALLID **ID**
Yes | `>` | `socketcall` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `socketcall` | SYSCALLID **ID**
Yes | `>` | `epoll_wait_old` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `epoll_wait_old` | SYSCALLID **ID**
Yes | `>` | `arch_prctl` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `arch_prctl` | SYSCALLID **ID**
Yes | `>` | `ssetmask` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ssetmask` | SYSCALLID **ID**
Yes | `>` | `sys_debug_setcontext` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sys_debug_setcontext` | SYSCALLID **ID**
Yes | `>` | `move_mount` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `move_mount` | SYSCALLID **ID**
Yes | `>` | `sigpending` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sigpending` | SYSCALLID **ID**
Yes | `>` | `lookup_dcookie` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `lookup_dcookie` | SYSCALLID **ID**
Yes | `>` | `olduname` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `olduname` | SYSCALLID **ID**
Yes | `>` | `fsopen` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fsopen` | SYSCALLID **ID**
Yes | `>` | `sched_get_priority_max` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_get_priority_max` | SYSCALLID **ID**
Yes | `>` | `signal` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `signal` | SYSCALLID **ID**
Yes | `>` | `nice` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `nice` | SYSCALLID **ID**
Yes | `>` | `map_shadow_stack` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `map_shadow_stack` | SYSCALLID **ID**
Yes | `>` | `modify_ldt` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `modify_ldt` | SYSCALLID **ID**
Yes | `>` | `_newselect` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `_newselect` | SYSCALLID **ID**
Yes | `>` | `stime` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `stime` | SYSCALLID **ID**
Yes | `>` | `waitpid` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `waitpid` | SYSCALLID **ID**
Yes | `>` | `sigaltstack` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sigaltstack` | SYSCALLID **ID**
Yes | `>` | `getrandom` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `getrandom` | SYSCALLID **ID**
Yes | `>` | `fadvise64` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fadvise64` | SYSCALLID **ID**
Yes | `>` | `fspick` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `fspick` | SYSCALLID **ID**
Yes | `>` | `pwritev2` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pwritev2` | SYSCALLID **ID**
Yes | `>` | `open_tree` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `open_tree` | SYSCALLID **ID**
Yes | `>` | `create_module` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `create_module` | SYSCALLID **ID**
Yes | `>` | `sched_setaffinity` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_setaffinity` | SYSCALLID **ID**
Yes | `>` | `sched_rr_get_interval` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_rr_get_interval` | SYSCALLID **ID**
Yes | `>` | `memfd_secret` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `memfd_secret` | SYSCALLID **ID**
Yes | `>` | `sched_getattr` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `sched_getattr` | SYSCALLID **ID**
Yes | `>` | `ioperm` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `ioperm` | SYSCALLID **ID**
Yes | `>` | `pkey_mprotect` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pkey_mprotect` | SYSCALLID **ID**
Yes | `>` | `membarrier` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `membarrier` | SYSCALLID **ID**
Yes | `>` | `pkey_free` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `pkey_free` | SYSCALLID **ID**
Yes | `>` | `landlock_restrict_self` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `landlock_restrict_self` | SYSCALLID **ID**
Yes | `>` | `landlock_add_rule` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `landlock_add_rule` | SYSCALLID **ID**
Yes | `>` | `kcmp` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `kcmp` | SYSCALLID **ID**
Yes | `>` | `statx` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `statx` | SYSCALLID **ID**
Yes | `>` | `set_mempolicy` | SYSCALLID **ID**, UINT16 **nativeID**
Yes | `<` | `set_mempolicy` | SYSCALLID **ID**

## Tracepoint events

Default | Dir | Name | Params
:-------|:----|:-----|:-----
Yes | `>` | `switch` | PID **next**, UINT64 **pgft_maj**, UINT64 **pgft_min**, UINT32 **vm_size**, UINT32 **vm_rss**, UINT32 **vm_swap**
Yes | `>` | `procexit` | ERRNO **status**, ERRNO **ret**, SIGTYPE **sig**, UINT8 **core**, PID **reaper_tid**
Yes | `>` | `signaldeliver` | PID **spid**, PID **dpid**, SIGTYPE **sig**
Yes | `>` | `page_fault` | UINT64 **addr**, UINT64 **ip**, FLAGS32 **error**: *PROTECTION_VIOLATION*, *PAGE_NOT_PRESENT*, *WRITE_ACCESS*, *READ_ACCESS*, *USER_FAULT*, *SUPERVISOR_FAULT*, *RESERVED_PAGE*, *INSTRUCTION_FETCH*

## Plugin events

Default | Dir | Name | Params
:-------|:----|:-----|:-----
Yes | `>` | `pluginevent` | UINT32 **plugin_id**, BYTEBUF **event_data**

## Metaevents

Default | Dir | Name | Params
:-------|:----|:-----|:-----
Yes | `>` | `drop` | UINT32 **ratio**
Yes | `<` | `drop` | UINT32 **ratio**
Yes | `>` | `scapevent` | UINT32 **event_type**, UINT64 **event_data**
Yes | `>` | `procinfo` | UINT64 **cpu_usr**, UINT64 **cpu_sys**
Yes | `>` | `cpu_hotplug` | UINT32 **cpu**, UINT32 **action**
Yes | `>` | `k8s` | CHARBUF **json**
Yes | `>` | `tracer` | INT64 **id**, CHARBUFARRAY **tags**, CHARBUF_PAIR_ARRAY **args**
Yes | `<` | `tracer` | INT64 **id**, CHARBUFARRAY **tags**, CHARBUF_PAIR_ARRAY **args**
Yes | `>` | `mesos` | CHARBUF **json**
Yes | `>` | `notification` | CHARBUF **id**, CHARBUF **desc**
Yes | `>` | `infra` | CHARBUF **source**, CHARBUF **name**, CHARBUF **description**, CHARBUF **scope**
Yes | `>` | `container` | CHARBUF **json**
Yes | `>` | `useradded` | UINT32 **uid**, UINT32 **gid**, CHARBUF **name**, CHARBUF **home**, CHARBUF **shell**, CHARBUF **container_id**
Yes | `>` | `userdeleted` | UINT32 **uid**, UINT32 **gid**, CHARBUF **name**, CHARBUF **home**, CHARBUF **shell**, CHARBUF **container_id**
Yes | `>` | `groupadded` | UINT32 **gid**, CHARBUF **name**, CHARBUF **container_id**
Yes | `>` | `groupdeleted` | UINT32 **gid**, CHARBUF **name**, CHARBUF **container_id**
Yes | `>` | `asyncevent` | UINT32 **plugin_id**, CHARBUF **name**, BYTEBUF **data**
