---
linkTitle: "Scanning (Legacy)"
title: "Scanning (Legacy)"
weight: "50"
aliases:
  - /en/scanning.html
  - /en/legacy-scan
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

### Two Types of Scanning

**As of May 2021,** Sysdig Secure includes two different types of scanning
for vulnerabilities:

- *Image scanning* This includes all prior scanning tools, policies,
    alerts, etc. in Sysdig Secure and focuses on *scanning the container images* in an environment.

- [Host scanning](/en/docs/sysdig-secure/vulnerabilities/scanning/host-scanning/#host-scanning):*(New)* This feature, deployed via the [Node
    Analyzer](/en/install-reqs-host-scan), *scans the host operating system*, whether OS (e.g `rpm`, `dpkg`) or non-OS
    (e.g. Java packages, Ruby gems).

{{% callout type="note" %}}
Host scanning documentation is self-contained; the rest of the topics in
this Scanning module concern image scanning.

{{% /callout %}}

## How Sysdig Image Scanning Works

Image scanning allows you to scan container images for vulnerabilities,
secrets, license violations, and more. It can be used as part of a
development build process, can validate images added to your container
registry, and can scan the images used by running containers on your
infrastructure.

The basic set up for image scanning is simple: provide registry
information where your images are stored, trigger a scan, and review the
results.

Behind the scenes:

- Image contents are analyzed.

- The contents report is evaluated against multiple vulnerability
    databases.

- It is then compared against default or user-defined policies.

- Results are reported, both in Sysdig Secure and (if applicable) in a
    developer's external CI tool.

### Prerequisites

- **Network and port requirements**

    Image Scanning requires access to an external vulnerability feed. To
    ensure proper access to the latest definitions, refer to the
    [Network and Port](/en/docs/administration/on-premises-deployments/architecture-system-requirements/system-requirements/#system-requirements)
    requirements.

- **Whitelisted IP for image scanning requests**

    Image scanning requests and Splunk event forwards both originate
    from **18.209.200.129**. To enable Sysdig to scan private
    repositories, your firewall will need to allow inbound requests from
    this IP address.

### Image Contents Reported

The analysis generates a detailed report of the image contents,
including:

- Official OS packages

- Unofficial OS packages

- Configuration files

- Credentials files

- Localization modules and software-specific installers:

  - Javascript with NPM

  - Python PiP

  - Ruby with GEM

  - Java/JVM with .jar archives

- Image metadata and configuration attributes

### Vulnerability Databases Used

Sysdig Secure continuously checks against a wide range of vulnerability
databases, updating the Runtime scan results with any newly detected
CVEs.

The current database list includes:

<table>
<tbody>
<tr class="odd">
<td><p><img src="/image/374670439.png" height="150" /></p>
<p><a href="https://www.cvedetails.com/vulnerability-list/vendor_id-10167/product_id-18131/Centos-Centos.html"> Centos</a> <a href="https://security-tracker.debian.org/tracker/">Debian</a> <a href="https://www.ruby-lang.org/en/security/">Ruby</a> <a href="https://www.redhat.com/security/data/metrics/">Red Hat</a> <a href="https://people.canonical.com/~ubuntu-security/cve/">Ubuntu</a> <a href="https://www.cvedetails.com/vulnerability-list/vendor_id-10210/product_id-18230/Python-Python.html">Python</a></p>
<p><a href="https://www.cvedetails.com/">CVE</a> <a href="https://nvd.nist.gov/">NIST</a> <a href="https://www.npmjs.com/advisories">NPM</a> <a href="https://www.cvedetails.com/vulnerability-list/vendor_id-16697/product_id-38838/Alpinelinux-Alpine-Linux.html">Alpine</a> <a href="https://nvd.nist.gov/">NVD</a> <a href="https://vulndb.cyberriskanalytics.com/">VulnDB</a></p>
<p>See also: <a href="/en/docs/administration/on-premises-deployments/on-premises-installation/installer-kubernetes-openshift/#updating-vulnerability-feed-in-airgapped-environments">Updating Vulnerability Feed in Airgapped Environments</a>.</p></td>
</tr>
</tbody>
</table>

## Use Cases

As an organization, you define what is an acceptable, secure, reliable
image running in your environment. Image scanning for the development
pipeline follows a somewhat different flow than for security personnel.

### Scanning During Container Development (DevOps)

[Use image scanning as part of your development
pipeline,](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-cicd-tools/#integrate-with-cicd-tools) to check for best
practices, vulnerabilities, and sensitive content.

To begin:

- **Add Registry:** Add a registry where your images are stored, along
    with the credentials necessary to access them.

- **Integrate CI Tool:** Integrate image scanning with an external CI
    tool, using the Jenkins plugin or building your own integration from
    a SysdigLabs solution.

- **Scan Image(s):** The plugin or CLI integration triggers the image
    scanning process. Failed builds will be stopped, if so configured.

- **Review Results (in CI tool):** Developers can analyze the results
    in the integrated CI tool (Jenkins).

    (Optionally: add policies or refine the default policies to suit
    your needs, assign policies to particular images or tags, and
    configure alerts and notifications.)

### Scanning Running Containers (Security Personnel)

Security personnel uses image scanning to monitor which containers are
running, what their scan status is, and whether new vulnerabilities are
present in their images.

- **Add Registry:** Add a registry where your images are stored, along
    with the credentials necessary to access them.

- **Scan Image(s):** Trigger an image scan with the node image
    analyzer or manually (one-by-one).

- **Review Results (in Sysdig Secure):** Security personnel can
    analyze scan results in the Sysdig Secure image scanning UI.

    (Optionally: add policies or refine the default policies to suit
    your needs, assign policies to particular images or tags, and
    configure alerts and notifications.)

{{% callout type="note" %}}

Image Scanning requires access to an external vulnerability feed. To
ensure proper access to the latest definitions, refer to the [Network
and Port](/en/docs/administration/on-premises-deployments/architecture-system-requirements/system-requirements/#system-requirements) requirements.

{{% /callout %}}

#### Add Scanning to Container Registries

In some cases, it is possible to integrate image scanning directly into
a container registry and automatically trigger an event or action every
time a new container is pushed into the registry. This feature is
currently supported for the following container registry:

- [AWS ECR](/en/docs/sysdig-secure/vulnerabilities/scanning/integrate-with-container-registries/#integrate-with-container-registries)
