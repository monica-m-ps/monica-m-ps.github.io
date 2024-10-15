---
linkTitle: "Sysdig Agent"
title: "Sysdig Agent"
weight: "10"
no_list: true
aliases:
  - /en/install-package-agent-secure
  - /en/docs/installation/sysdig-agent/agent-installation/agent-install-manual-linux-installation/
  - /en/docs/installation/sysdig-secure/install-agent-components/hosts/packages/sysdig-agent/
description: "You can install Sysdig Agent as a Linux package on Debian, Ubuntu, CentOS, RHEL, Fedora, Amazon AMI, and Amazon Linux 2."
---

## Use the Quick Start Wizard

This option provides a bash script for installing the agent and is appropriate for quick trial installations to get Sysdig up and running.

1. Log in to Sysdig Secure as an administrator and select **Integrations > Data Sources|Sysdig Agent**.

2. Click **+Add Agent** and select **Linux**.

   ![](/image/wizard_package.png)

3. As prompted by the Wizard screen, enter:

   * **Tags:** Enter any tags you like, so you can identify this specific agent later.

4. The Wizard will auto-populate a code snippet with any tags, as well as autodetected Sysdig Secure endpoint and  [agent access key](/en/agent-access-key/) information.



5. Copy and run the script. This will install the Sysdig agent and give you runtime threat detection.

## Customized Deployments

### Prerequisites

- Review
   - [Installation Requirements](/en/install-requirements-monitor)
   - [Understand Agent Drivers](/en/understand-agent-drivers)
- Collect the following:
   - [Sysdig access key](/en/docs/administration/administration-settings/agent-installation-overview-and-key/)
   - [Collector address](/en/docs/administration/saas-regions-and-ip-ranges/)
- Ensure that you have admin permissions to perform the operations.

### Supported Distributions

Installing agent as a package is supported on the following :

- Debian, Ubuntu
- CentOS, RHEL, Fedora, Amazon AMI, Amazon Linux 2

{{% callout type="note" %}}

Starting with agent version 13.1.0, separate packages will have to be installed depending on the driver to be used.
See the table below.

{{% /callout %}}

#### Package Reference

| Driver                    | Main Package             | Dependency Packages                     |
|---------------------------|--------------------------|-----------------------------------------|
| kmod (compatibility mode) | draios-agent             | draios-agent-slim, draios-agent-kmodule |
| kmod (recommended)        | draios-agent-kmodule     | draios-agent-slim                       |
| legacy_ebpf               | draios-agent-legacy-ebpf | draios-agent-slim                       |
| universal_ebpf            | draios-agent-slim        |                                         |


### For Debian and Ubuntu

1. Trust the Sysdig GNU Privacy Guard (GPG) key, configure the apt repository, and update the package list by running the following commands:

   ```bash
   curl -s https://download.sysdig.com/DRAIOS-GPG-KEY.public | apt-key add -
   curl -o /etc/apt/sources.list.d/draios.list https://download.sysdig.com/stable/deb/draios.list
   apt-get update
   ```

2. Install kernel development files, (kernel module and eBPF drivers, only):

   ```Shell
   sudo apt-get -y install linux-headers-$(uname -r)
   ```

3.  Install, configure, and restart the Sysdig agent:
- Install the agent:
  ```Shell
  sudo apt-get -y install draios-agent
  ```
- Specify the agent driver:
   - To select the eBPF driver:
     ```Shell
     cat > /etc/default/dragent <<< 'export SYSDIG_BPF_PROBE=""'
     cat >> /etc/default/dragent <<< "SYSDIG_AGENT_DRIVER=legacy_ebpf"
     ```
   - To select the Universal eBPF driver:
     ```Shell
     cat > /etc/default/dragent <<< "SYSDIG_AGENT_DRIVER=universal_ebpf"
     ```
   - To select the kernel module driver:
     ```Shell
     cat > /etc/default/dragent <<< "SYSDIG_AGENT_DRIVER=kmod"
     ```
     Note: On new installations, the kernel module driver is selected by default, and specifying it explicitly in `/etc/default/dragent` is optional.

- Configure `dragent.yaml`:
  ```Shell
  sudo bash -c `echo customerid: ACCESS_KEY >> /opt/draios/etc/dragent.yaml`
  sudo bash -c `echo tags: [TAGS] >> /opt/draios/etc/dragent.yaml`
  sudo bash -c `echo collector: COLLECTOR_ADDRESS >> /opt/draios/etc/dragent.yaml`
  ```
  Replace `ACCESS_KEY` and `COLLECTOR_ADDRESS` with the access key and collector address associated with your account. `<TAGS>` is optional and can be used to add custom tags to your metrics.

- Restart the agent:
  ```Shell
  sudo service dragent restart
  ```

### **For CentOS, RHEL, Fedora, Amazon AMI, Amazon Linux 2**

1. Trust the Sysdig GPG key and configure the yum repository:

   ```Shell
   sudo rpm --import https://download.sysdig.com/DRAIOS-GPG-KEY.public && sudo curl -s -o /etc/yum.repos.d/draios.repo https://download.sysdig.com/stable/rpm/draios.repo
   ```

2. Install the [EPEL](https://docs.fedoraproject.org/en-US/epel/) repository, (kernel module and eBPF drivers, only):

   ```Shell
   sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
   ```

   This command is required only if [DKMS](https://en.wikipedia.org/wiki/Dynamic_Kernel_Module_Support) is not available in the base distribution.

3. Install the kernel development files, (kernel module and eBPF drivers, only):

   ```Shell
   sudo yum -y install kernel-devel-$(uname -r)
   ```

4. Install, configure, and start the Sysdig Agent:
   - Install the agent:
     ```Shell
     yum -y install draios-agent
     ```
   - Specify the agent driver:
      - To select the kernel module driver:
        ```Shell
        cat > /etc/sysconfig/dragent <<< "SYSDIG_AGENT_DRIVER=kmod"
        ```
        Note: On new installations, the kernel module driver is selected by default, and specifying it explicitly in `/etc/sysconfig/dragent` is optional.
      - To select the <i>eBPF</i> driver
        ```Shell
        cat > /etc/sysconfig/dragent <<< 'export SYSDIG_BPF_PROBE=""'
        cat >> /etc/sysconfig/dragent <<< "SYSDIG_AGENT_DRIVER=legacy_ebpf"
        ```
      - To select the Universal eBPF driver:
        ```Shell
        cat > /etc/sysconfig/dragent <<< "SYSDIG_AGENT_DRIVER=universal_ebpf"
        ```
   - Configure `dragent.yaml`:
     ```Shell
     echo customerid: ACCESS_KEY >> /opt/draios/etc/dragent.yaml
     echo tags: [TAGS] >> /opt/draios/etc/dragent.yaml
     echo collector: COLLECTOR_ADDRESS >> /opt/draios/etc/dragent.yaml
     ```
     Replace `ACCESS_KEY` and `COLLECTOR_ADDRESS` with your own configuration parameters. `[TAGS]` is optional and can be used to add custom tags to the agent's metrics.  For example, `env:production`, `cluster:east-cluster-a`.
   - Start the agent:
     ```Shell
     sudo systemctl enable dragent
     sudo systemctl start dragent
     ```

## Next Steps

[Install the Host Scanner using a package](/en/install-package-host-scanner)
