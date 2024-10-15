---
linkTitle: "Sysdig Agent"
title: "Sysdig Agent"
weight: "10"
no_list: true
aliases:
  - /en/install-container-agent-secure
description: Sysdig supports installing the Sysdig Agent using containers.
---

## Prerequisites

- Review
  - [Installation Requirements](/en/install-reqs-agent-secure/)
  - [Understand Agent Drivers](/en/understand-agent-drivers)
- Collect the following:
  - [Sysdig access key](/en/docs/administration/administration-settings/agent-installation-overview-and-key/)
  - [Collector address](/en/docs/administration/saas-regions-and-ip-ranges/)

For more information on agent configuration, see [Configure Sysdig Agent](/en/configure-sysdig-agent).

## Use the Quick Start Wizard

This option provides a script for installing the agent and is appropriate for quick trial installations to get Sysdig up and running. 

1. Log in to Sysdig Secure as `admin` and select **Integrations > Data Sources|Sysdig Agent**.

2. Click **+Add Account** and select **Docker**. 

   ![](/image/wizard_container.png)

3. As prompted by the Wizard screen, enter:

   * **Tags:** For identifying your container installation.

4. The Wizard will auto-populate a code snippet with autodetected Sysdig Secure endpoint and [agent access key](/en/agent-access-key/) information. 

5. Copy and run the script. This will install the Sysdig agent and give you runtime threat detection. 

## Customized Deployment

This option can be integrated with your enterprise deployment methods at a production scale. 

{{% callout type="note" %}}
* GKE COS environments require the eBPF or Universal eBPF driver to run the Sysdig Agent.
* Agent versions 12.17.0 and newer ship with a pre-built Universal eBPF object embedded in the agent binary.  Running the `sysdig-agent-kmodule` container is not necessary when using the Universal eBPF driver.
{{% /callout %}}

1. Build and load the kernel module or eBPF object file, kernel module and eBPF drivers, only:

   - If you are using the kernel module, driver, run:

     ```Shell
     docker run -it --privileged --rm --name sysdig-agent-kmodule \
       -v /usr:/host/usr:ro \
       -v /boot:/host/boot:ro \
       -v /lib/modules:/host/lib/modules \
       quay.io/sysdig/agent-kmodule
     ```

   - If you are using the eBPF driver, run:

     ```Shell
     docker run -it --privileged --rm --name sysdig-agent-kmodule \
       -e SYSDIG_BPF_PROBE="" \
       -v /usr:/host/usr:ro \
       -v /boot:/host/boot:ro \
       -v /lib/modules:/host/lib/modules:ro \
       -v /etc/os-release:/host/etc/os-release:ro \
       -v /root/.sysdig:/root/.sysdig \
       quay.io/sysdig/agent-kmodule
     ```
  - If you are using Universal eBPF, skip to step 3.

2. Configure the kernel module to load during system boot, skip for eBPF and Universal eBPF:

    ```Shell
    sudo mkdir -p /etc/modules-load.d
    sudo bash -c "echo sysdigcloud-probe > /etc/modules-load.d/sysdigcloud-probe.conf"
    ```

3. Run the agent container providing the access key and, optionally, user-defined tags:

  - If you are using kernel module, run:

    ```Shell
    docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>] \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      -v /boot:/host/boot:ro \
      --shm-size=512m \
      quay.io/sysdig/agent-slim
    ```

  - If you are using eBPF, run:

    ```Shell
    docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e SYSDIG_BPF_PROBE="" \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS> ] \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      -v /boot:/host/boot:ro \
      -v /sys/kernel/debug:/sys/kernel/debug:ro \
      -v /root/.sysdig:/root/.sysdig \
      --shm-size=512m \
      quay.io/sysdig/agent-slim
    ```

  - If you are using Universal eBPF, run:
    ```Shell
    docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e SYSDIG_AGENT_DRIVER=universal_ebpf \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS> ] \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /proc:/host/proc:ro \
      -v /sys/kernel/debug:/sys/kernel/debug:ro \
      --shm-size=512m \
      quay.io/sysdig/agent-slim
      ```


      Replace `<ACCESS_KEY>` and `<COLLECTOR_ADDRESS>` with the access key and collector address collected in the prerequisites. `<TAGS>` is optional.

4. Verify that the Sysdig Agent is running by running the following command:
    ```Shell
    docker ps
    ```
  
    You should see the `sysdig-agent` container listed in the output.

For additional configuration options, including on-premise, using a proxy, etc.,  see [**Agent Configuration**](/en/configure-sysdig-agent). 

## Install As a Single Container (Legacy)

The legacy way of installing an agent involves running it as a single container. It includes the components for downloading and building the kernel module, as well as for gathering and reporting on a wide variety of pre-defined metrics and events. 

### SaaS

1. Collect the [necessary environment variables](#common-environment-variables-for-agent-containers).

2. Run the agent container providing the access key and, optionally, user-defined tags:

    If you are using kernel module, use the following:

      ```Shell
      docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>] \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      -v /boot:/host/boot:ro \
      -v /lib/modules:/host/lib/modules:ro \
      -v /usr:/host/usr:ro \
      --shm-size=512m \
      -v /etc/modprobe.d:/etc/modprobe.d \
      quay.io/sysdig/agent
      ```

    If you are using eBPF use the following:

      ```Shell
      docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>] \
      -e SYSDIG_BPF_PROBE="" \
      -v /sys/kernel/debug:/sys/kernel/debug:ro \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      -v /boot:/host/boot:ro \
      -v /lib/modules:/host/lib/modules:ro \
      -v /usr:/host/usr:ro \
      --shm-size=512m \
      quay.io/sysdig/agent
      ```

   If you are using Universal eBPF, use the following:

      ```Shell
      docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>] \
      -e SYSDIG_AGENT_DRIVER=universal_ebpf \
      -v /sys/kernel/debug:/sys/kernel/debug:ro \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      --shm-size=512m \
      quay.io/sysdig/agent-slim
      ```

### On-Premises 

1. Collect the [necessary environment variables](#common-environment-variables-for-agent-containers).

2. Run the agent container providing the access key and, optionally, user-defined tags:

      If you are using kernel module, run:

      ```Shell
      docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>]
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      -v /boot:/host/boot:ro \
      -v /lib/modules:/host/lib/modules:ro \
      -v /usr:/host/usr:ro \
      --shm-size=512m \
      quay.io/sysdig/agent
      ```

      If you are using eBPF, run:

      ```Shell
      docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>]
      -e SYSDIG_BPF_PROBE="" \
      -v /sys/kernel/debug:/sys/kernel/debug:ro \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      -v /boot:/host/boot:ro \
      -v /lib/modules:/host/lib/modules:ro \
      -v /usr:/host/usr:ro \
      --shm-size=512m \
      quay.io/sysdig/agent
      ```
      
      If you are using Universal eBPF, run:

      ```Shell
      docker run -d --name sysdig-agent \
      --restart always \
      --privileged \
      --net host \
      --pid host \
      -e ACCESS_KEY=<ACCESS_KEY> \
      -e COLLECTOR=<COLLECTOR_ADDRESS> \
      [-e TAGS=<TAGS>]
      -e SYSDIG_AGENT_DRIVER=universal_ebpf \
      -v /sys/kernel/debug:/sys/kernel/debug:ro \
      -v /var/run/docker.sock:/host/var/run/docker.sock \
      -v /dev:/host/dev \
      -v /proc:/host/proc:ro \
      --shm-size=512m \
      quay.io/sysdig/agent-slim
      ```

### Common Environment Variables for Agent Containers

| Option            | Description                                                  |
|-------------------| ------------------------------------------------------------ |
| `ACCESS_KEY`      | The agent access key. You can retrieve this from Settings > Agent Installation in either Sysdig Monitor or Sysdig Secure. |
| `TAGS`            | The list of tags for the host where the agent is installed. For example: `role:webserver`, `location:europe` |
| `COLLECTOR`       | The collector URL for Sysdig Monitor or Sysdig Secure. This value is region-dependent in SaaS and is auto-completed on the Get Started page in the Monitor UI or Data Sources page in Secure.  It is a custom value in on-prem installations. See [SaaS Regions and IP Ranges](/en/docs/administration/saas-regions-and-ip-ranges/#saas-regions-and-ip-ranges). |
| `COLLECTOR_PORT`  | The default is 6443.                                         |
| `ADDITIONAL_CONF` | Optional. Use this option to provide custom configuration values to the agent as environment variables. If provided, will be appended to the agent configuration file.  |
| `SYSDIG_AGENT_DRIVER` | Optional. The syscall capture driver that is used by the agent.  Valid values are `kmod`, `universal_ebpf`, and `legacy_ebpf`. Agent defaults to `kmod` if this environment variable is not set |
| `SYSDIG_BPF_PROBE` | <p>Optional. Deprecated and superseded by <code>SYSDIG_AGENT_DRIVER</code>.  The old environment variable that is used to force the agent to load the current eBPF driver.  Valid values are `""` or a path within the container to a compatible eBPF object file.</p><p><b><i>Note:</i></b>The agent will exit with an error if <code>SYSDIG_AGENT_DRIVER</code> and <code>SYSDIG_BPF_PROBE</code> are set to conflicting values. |

See [Understand the Agent Configuration](/en/understand-the-agent-configuration) for additional information on agent and container environment variables.

### Mount Points

Given below is the list of all the volumes that need to be shared from the agent and the host. For each container, you can see the volumes in the `source:destination` format.

#### Volumes

- `/dev`: Required to interact with the ring buffer in the kmodule.

- `/proc`: Required to get information about the current processes running in the host.

- `/etc`: Required to get certain information about the system. The os-release to detect certain distros and `passwd` / `group` to get `uids` and `gids`.

- `/etc/modprobe.d`: Identify the kernel modules available. See the [manpage](https://manpages.ubuntu.com/manpages/trusty/man5/modprobe.d.5.html) for more information.

- `/boot`: Contains the information related to the kernel module required to compile the drivers or get the proper kernel hash if you need to download the driver from the CDN.

- `/lib/modules`: Contains the kernel module build environment.

- `/usr`: Gets `/usr/src/linux` for the kernel sources.

- `/run`: Some systems use this instead of `/var/run`.

- `/var/lib`: Provides visibility into container filesystems.

- `/var/run`: This is where the `cri` socket is stored for interacting with the CRI container runtime.

- `/root/.sysdig`: The directory where the ko file that you build is stored when you need to build the eBPF module that needs to be loaded by the agent.

- `/sys/kernel/debug`: Required to expose information available in the kernel into user space. See [debugfs](https://docs.kernel.org/filesystems/debugfs.html) for more information.

#### Always

- `/dev:/host/dev`
- `/proc:/host/proc`
- `/dev/shm:/dev/shm`

#### Agent Container with Kernel Module

Container image on Quay: [https://quay.io/sysdig/agent](https://quay.io/sysdig/agent)

- `/etc/modprobe.d:/etc/modprobe.d` (read-only mode)

- `/etc:/host/etc `(read-only mode)

- `/boot:/host/boot` (read-only mode)

- `/lib/modules:/host/lib/modules` (read-only mode)

- `/usr:/host/usr` (read-only mode)

- `/run:/host/run`

- `/var/lib:/host/var/lib` (read-only mode)

- `/var/run:/host/var/run`


#### Agent Container with eBPF

Container image on Quay [https://quay.io/sysdig/agent](https://quay.io/sysdig/agent) 

- `/etc/modprobe.d:/etc/modprobe.d` (read-only mode)

- `/etc:/host/etc` (read-only mode)

- `/boot:/host/boot` (read-only mode)

- `/lib/modules:/host/lib/modules` (read-only mode)

- `/usr:/host/usr` (read-only mode)

- `/run:/host/run`

- `/var/lib:/host/var/lib` (read-only mode)

- `/var/run:/host/var/run`

- `/sys/kernel/debug:/sys/kernel/debug` (read-only mode)

#### Slim Agent Container with Kernel Module

Container image on Quay [https://quay.io/sysdig/agent](https://quay.io/sysdig/agent-slim) 

- `/etc:/host/etc` (read-only mode)

- `/run:/host/run`

- `/var/lib:/host/var/lib` (read-only mode)

- `/var/run:/host/var/run`

#### Slim Agent Container with eBPF or Universal eBPF 

Container image on Quay [https://quay.io/sysdig/agent](https://quay.io/sysdig/agent-slim) 

- `/etc:/host/etc` (read-only mode)

- `/run:/host/run`

- `/var/lib:/host/var/lib` (read-only mode)

- `/var/run:/host/var/run`

- `/sys/kernel/debug:/sys/kernel/debug` (read-only mode)

#### Agent Kmodule Container with Kernel Module

Container image on Quay [https://quay.io/sysdig/agent](https://quay.io/sysdig/agent-kmodule) 

- `/etc/modprobe.d:/etc/modprobe.d` (read-only mode)

- `/etc:/host/etc` (read-only mode)

- `/boot:/host/boot` (read-only mode)

- `/lib/modules:/host/lib/modules` (read-only mode)

- `/usr:/host/usr` (read-only mode)

#### Agent Kmodule Container with eBPF

Container image on Quay [https://quay.io/sysdig/agent](https://quay.io/sysdig/agent-kmodule) 

- `/etc/modprobe.d:/etc/modprobe.d` (read-only mode)

- `/etc:/host/etc` (read-only mode)

- `/boot:/host/boot` (read-only mode)

- `/lib/modules:/host/lib/modules` (read-only mode)

- `/usr:/host/usr` (read-only mode)

- `/root/.sysdig:/root/.sysdig`
  
  Shares the eBPF driver between the container that builds the driver and the container that loads the driver

- `/sys/kernel/debug:/sys/kernel/debug` (read-only mode)

## Next Steps

[Install the Host Scanner as a container](/en/install-container-host-scanner)

[Install the Rapid Response as a container](/en/install-container-rapid-response/)
