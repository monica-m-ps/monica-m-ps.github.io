---
linkTitle: "Kernel Header Troubleshooting"
title: "Kernel Header Troubleshooting"
weight: "11"
aliases:
  - /en/kernel-header-troubleshooting.html
  - /en/kernel-header-troubleshooting
  - /en/docs/installation/sysdig-agent/agent-troubleshooting/kernel-header-troubleshooting/
Description: "This section describes how the agent uses kernel headers and provides you tips on troubleshooting, if needed."
---

## About Kernel Headers and the Kernel Module

The Sysdig agent requires a kernel module in order to install
successfully on a host. This can be obtained in three ways:

1.  **Agent compiles the module using kernel headers.**

    If the hosts in your environment already have kernel header files
    pre-installed, no special action is needed. Or you can install the
    kernel headers manually; see below.

2.  **Agent auto-downloads precompiled modules from Sysdig's AWS storage
    location.**

    If the headers are not already installed but the agent is able to
    auto-download, no special action is needed. If there is no internet
    connectivity, you can use method 3 (download from an internal URL).

3.  **Agent downloads precompiled modules from an internal URL.**

    Use the environment variable `SYSDIG_PROBE_URL`. See also
    [Understanding the Agent Config
    Files](/en/configure-sysdig-agent). Contact Sysdig
    support for assistance.



## Agent Installation on SELinux-Enabled Linux Distributions

On Fedora 35 or similar SELinux-enabled distributions with default restrictive policies, the agent init container, `agent-kmodule`, will not install the downloaded kernel module raising an error similar to the following:

`insmod: ERROR: could not insert module /root/.sysdig/sysdigcloud-probe-12.3.1-x86_64-5.16.11-200.fc35.x86_64-67098c7fdcc97105d4b9fd0bb2341888.ko: Permission denied`

In such cases, we recommend that you use eBPF option while running `agent-kmodule` instead.


## TracePoints

All supported distribution released kernels have this support but if
creating a custom kernel, it must support the following options:

- `CONFIG_TRACEPOINTS `
- `CONFIG_HAVE_SYSCALL_TRACEPOINTS`

### To Install Kernel Headers

In some cases, the host(s) in your environment may use Unix versions
that do not match the provided headers, and the agent may fail to
install correctly. In those cases, you must install the kernal headers
manually.

#### Debian-Style

For Debian-syle distributions, run the command:

```yaml
apt-get -y install linux-headers-$(uname -r)
```

#### RHEL-Style

For RHEL-style distributions, run the command:

```yaml
yum -y install kernel-devel-$(uname -r)
```

#### RancherOS

For RancherOS distributions, the kernel headers are available in the
form of a system service and therefore are enabled using the
`ros `service command:

```yaml
sudo ros service enable kernel-headers-system-docker
sudo ros service up -d kernel-headers-system-docker
```

NOTE: Some cloud hosting service providers supply pre-configured Linux
instances with customized kernels. You may need to contact your
provider's support desk for instructions on obtaining appropriate header
files, or for installing the distribution's default kernel.

### To Correct Kernel Header Errors in AWS AMI

During an agent installation in an Amazon machine image (AMI) you may
encounter the following errors while the installer is trying to compile
the Sysdig kernel module:

**Errors**

-   *"Unable to find kernel development files for the current kernel
    version*" or

-   *"FATAL: Module sysdigcloud-probe not found"*

This indicates your machine is running a kernel in an older AMI for
which the kernel headers are no longer available in the configured
repositories. The issue has to do with Amazon purging packages in the
yum repository when new Amazon Linux machine images are released.

The solution is either to update your kernel to a version for which
header files are readily available (recommended), or perform a one-time
installation of the kernel headers for your older AMI.

#### Option 1: Upgrade Your Host's Kernel

First install a new kernel and reboot your instance:

```yaml
sudo yum -y install kernel
sudo reboot
```

After rebooting, check to see if the host is reporting metrics to your
Sysdig account. If not, you may need to issue three more commands to
install the required header files:

```yaml
sudo yum -y install kernel-devel-$(uname -r)
sudo /usr/lib/dkms/dkms_autoinstaller start
sudo service dragent restart
```

#### Option 2: Install Older Kernel Headers

Although it is recommended to upgrade to the latest kernel for security
and performance reasons, you can alternatively install the older headers
for your AMI.

Find the the AMI version string and install the appropriate headers with
the commands:

```yaml
releasever=$(cat /etc/os-release | grep 'VERSION_ID' | grep -Eo "[0-9]{4}\.[0-9]{2}")
sudo yum -y --releasever=${releasever} install kernel-devel-$(uname -r)
```

Issue the remaining commands to allow the Sysdig Agent to start
successfully:

```yaml
sudo /usr/lib/dkms/dkms_autoinstaller start
sudo service dragent restart
```

#### Reference: Find Your AWS Instance Image Version

The file `/etc/image-id` shows information about the original machine
image with which your instance was set up:

```yaml
[ec2-user ~]$ cat /etc/image-id
image_name="amzn-ami-hvm"
image_version="2017.03"
image_arch="x86_64"
image_file="amzn-ami-hvm-2017.03.0.20170401-x86_64.ext4.gpt"
image_stamp="26a3-ed31"
image_date="20170402053945"
recipe_name="amzn ami"
recipe_id="47cfa924-413c-d460-f4f2-2af7-feb6-9e37-7c9f1d2b"
```

This file will not change as you install updates from the yum
repository.

The file `/etc/system-release` will tell what version of the AWS image
is currently installed:

```yaml
[ec2-user ~]$ cat /etc/system-release
Amazon Linux AMI release 2017.03
