---
linkTitle: "Container Scanning"
title: "Container Scanning"
weight: "11"
aliases:
  - /en/nonk8s-container-scan
  - /en/container-scan
description: "To manage container vulnerabilities in your runtime environment, you can extend the Host Scanner to scan for Docker and Podman containers present within the host file system. Sysdig provides a list of vulnerabilities, policy evaluations, has-fix, and has-exploit information to help you focus on the most critical vulnerabilities in your environment."
---


Sysdig supports the following container scanning types :

* **Kubernetes containers:** The runtime scanner installed with the Sysdig agent scans for runtime vulnerabilities in Kubernetes workloads.
* **Non-Kubernetes containers:** To scan non-Kubernetes containers such as Docker, you can extend any of the three non-Kubernetes host scanning configurations, as described below.

## Prerequisites

* Sysdig Secure SaaS, running the Vulnerability Management engine
  * If you suspect you are on the old Scanning engine, see  [Which Scanning Engine to Use](/en/docs/sysdig-secure/vulnerabilities/scanning/new-scanning-engine/).
* Host Scanner v.0.7.0+

### Supported Container Versions

* Docker Engine API Version v1.21 (introduced in Docker Engine 1.9.0) and above.
* Podman version 3.1+

### Limitations

Risk Spotlight/In-Use and Reporting features are not yet supported for non-Kubernetes container scanning.

## Install on a Host as a Container

To install on a host as a container:

1. Run the following Docker command to deploy the Sysdig Host Scanning container:

   ```
   docker run --detach -e HOST_FS_MOUNT_PATH=/host -e SYSDIG_ACCESS_KEY=<access-key> -e SYSDIG_API_URL=<sysdig-secure-endpoint> -e USE_COMBINED_SCANNER=true  -e SCAN_CONTAINERS_ENABLED=true -e SCAN_ON_START=true -v /:/host:ro -v /var/run:/host/var/run:ro --uts=host --net=host quay.io/sysdig/vuln-host-scanner:$(curl -L -s https://download.sysdig.com/scanning/sysdig-host-scanner/latest_version.txt)
   ```

   This command downloads and starts the Sysdig Host Scanning container.

   * Replace *<access-key>* with your agent access key, and *<sysdig-secure-endpoint>* with the URL for your Sysdig Secure endpoint by region.

   Once the container is running, the scanner begins scanning your host for vulnerabilities and providing security recommendations. You can view the results in the Sysdig Secure UI after 12 hours of installation as scans are refreshed every 12 hours.

   Container scans will be shown within 30 minutes of installation. They are refreshed four times per day if new vulnerabilities are added to Sysdig's vulnerability database.

   By default, the host scanner attempts to connect to the following sockets:
   * Docker Unix socket `/var/run/docker.sock`  
   * Podman Unix socket `/var/run/podman.sock`

2. (Optional) If you have a custom socket location, you can override it by setting a custom socket location with environment variables:

   ```
   -e DOCKER_SOCKET_PATHS=unix:///var/run/docker.sock 
   -e PODMAN_SOCKET_PATHS=unix:///var/run/podman.sock
   ```

## Install as an RPM Package

To install as an RPM package:

1. Configure the RPM repository and Sysdig GPG key:

   ```
   sudo rpm --import https://download.sysdig.com/DRAIOS-GPG-KEY.public
   sudo curl -o /etc/yum.repos.d/draios.repo https://download.sysdig.com/stable/rpm/draios.repo
   ```

2. Install the vuln-host-scanner package:

   ```
   sudo yum install vuln-host-scanner --refresh -y
   ```

Note:  On RHEL/CentOS platforms, use ``sudo yum clean expire-cache && sudo yum install vuln-host-scanner -y``

3. Create the vuln-host-scanner configuration file:

   ```
   cat << EOF | sudo tee /opt/draios/etc/vuln-host-scanner/env
   SYSDIG_ACCESS_KEY=<access-key>
   SYSDIG_API_URL=<api-url>
   # optional
   SCAN_ON_START=true
   # container scanning options
   USE_COMBINED_SCANNER=true
   SCAN_CONTAINERS_ENABLED=true
   # optional container scanning parameters. 
   # Uncomment and provide them only if your docker / podman setup have a
   # different socket path
   # DOCKER_SOCKET_PATHS=unix:///var/run/docker.sock
   # PODMAN_SOCKET_PATHS=unix:///var/run/podman.sock
   EOF
   ```

4. Enable and start the vuln-host-scanner.service service:

   ```
   sudo systemctl enable --now vuln-host-scanner.service
   ```

5. Check logs to verify your configuration:

   ```
   sudo journalctl -fu vuln-host-scanner.service

## Install as a Binary Application

To install your container as a binary application:

1. Download the latest version of `sysdig-host-scanner` with:

   **Intel Processor (AMD64)**

   ```yaml
   curl -LO "https://download.sysdig.com/scanning/bin/sysdig-host-scanner/$(curl -L -s https://download.sysdig.com/scanning/sysdig-host-scanner/latest_version.txt)/linux/amd64/sysdig-host-scanner"
   ```

   **ARM Processor (ARM64)**

   ```yaml
   curl -LO "https://download.sysdig.com/scanning/bin/sysdig-host-scanner/$(curl -L -s https://download.sysdig.com/scanning/sysdig-host-scanner/latest_version.txt)/linux/arm64/sysdig-host-scanner"
   ```

2. Optionally, you can check the sha256sum as follows:

   **Intel Processor (AMD64)**

   ```yaml
   sha256sum -c <(curl -sL "https://download.sysdig.com/scanning/bin/sysdig-host-scanner/$(curl -L -s https://download.sysdig.com/scanning/sysdig-host-scanner/latest_version.txt)/linux/amd64/sysdig-host-scanner.sha256")
   ```

   **ARM Processor (ARM64)**

   ```yaml
   sha256sum -c <(curl -sL "https://download.sysdig.com/scanning/bin/sysdig-host-scanner/$(curl -L -s https://download.sysdig.com/scanning/sysdig-host-scanner/latest_version.txt)/linux/arm64/sysdig-host-scanner.sha256")
   ```

3. Set the executable flag on the file:

   ```
   chmod +x ./sysdig-host-scanner
   ```

   You only need to download and set the executable once.

4. You can scan the host by running the `sysdig-host-scanner` command:

   ```
   SYSDIG_ACCESS_KEY=<access-key> SYSDIG_API_URL=<api-url> ./sysdig-host-scanner
   ```

Optionally, create an environment file to store the configuration and a `systemd` unit file to run the binary as a service:

   ```
   sudo mv ./sysdig-host-scanner /usr/local/bin/vuln-host-scanner
   sudo restorecon -Rv /usr/local/bin/vuln-host-scanner
   sudo mkdir -p /opt/draios/etc/vuln-host-scanner/

   cat << EOF | sudo tee /opt/draios/etc/vuln-host-scanner/env
   SYSDIG_ACCESS_KEY=<access-key>
   SYSDIG_API_URL=<api-url>
   # optional
   SCAN_ON_START=true
   EOF

   cat << EOF | sudo tee /etc/systemd/system/vuln-host-scanner.service
   [Unit]
   Description=Sysdig Vuln Host Scanner component

   [Service]
   EnvironmentFile=/opt/draios/etc/vuln-host-scanner/env
   ExecStart=/usr/local/bin/vuln-host-scanner

   [Install]
   WantedBy=multi-user.target
   EOF

   sudo systemctl daemon-reload
   sudo systemctl enable --now vuln-host-scanner.service
   ```

### Add Custom Sockets (Optional)

By default, the host scanner attempts to connect to the following sockets:

* Docker Unix socket `/var/run/docker.sock`  
* Podman Unix socket `/var/run/podman.sock`

If you have a custom socket location, you can override it by setting the following environment variables:

```
-e DOCKER_SOCKET_PATHS=unix:///var/run/docker.sock 
-e PODMAN_SOCKET_PATHS=unix:///var/run/podman.sock
```

as follows:

```
SYSDIG_ACCESS_KEY=<access-key> SYSDIG_API_URL=<api-url> USE_COMBINED_SCANNER=true SCAN_CONTAINERS_ENABLED=true DOCKER_SOCKET_PATHS=unix:///var/run/docker.sock PODMAN_SOCKET_PATHS=unix:///var/run/podman.sock ./sysdig-host-scanner
```

#### Environment File Option

If you are creating the environment file to store the configuration and a `systemd unit` file to run the binary as a service, add the following to the  `/opt/draios/etc/vuln-host-scanner/env` section:

```
\# optional container scanning parameters. 
\# Uncomment and provide them only if your docker / podman setup have a
\# different socket path
\# DOCKER_SOCKET_PATHS=unix:///var/run/docker.sock
\# PODMAN_SOCKET_PATHS=unix:///var/run/podman.sock
```

## Container Scanning via Agentless Host Scanning

To enable container scanning through Sysdig Agentless Host Scanning:

1. Connect your cloud account with Agentless Scanning enabled. See [Connect Cloud Accounts](/en/cloud-accounts-secure/)

2. Navigate to your cloud account under **Cloud Accounts** and locate your specific cloud provider.

3. Navigate to the individual account, select it, and enable the **Container Scanning** check box.
