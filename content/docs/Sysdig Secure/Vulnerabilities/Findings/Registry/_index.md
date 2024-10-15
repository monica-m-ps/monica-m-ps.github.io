---
linkTitle: "Registry"
title: "Registry"
weight: "11"
aliases:
  - /en/vuln-registry-scan/
  - /en/docs/sysdig-secure/vulnerabilities/registry/
description: "To manage vulnerabilities in your system effectively, integrate Sysdig Secure with your container registry. This integration adds a layer of defense between your pipeline and runtime environment. On the Sysdig Secure Registry page, you can scan repositories manually, add registries, and review scan results from both the registry scanner and the Scan Now instant image scanning to assess your image security posture."
lab_url: https://enablement.sysdig.com/registry-image-scanning-with-sysdig-secure
---

Registries are a fundamental stage in the lifecycle of container images. Container registries accumulate a large number of images, some of which are obsolete or no longer suitable for runtime, and registry scanning provides the necessary security to avoid degradation of the posture.  

See the [Sysdig Vulnerability Management cycle](/en/vuln-manage/) for the benefits of scanning in pipeline, registry, and runtime phases.

The Registry page allows you to: 

- Search and view the list of integrated registries and repositories.
- Determine the overall vulnerability score and exploits.
- Drill down the images to view the security posture of associated packages and versions.

## Prerequisites

- To use **Registry Scanning**, install the Registry Scanner and configure it on your private registries. See [Install Registry Scanning](/en/install-registry-scan/).

### On-Prem Deployments

- **Network and port requirements:** Ensure port **443*** is open for outbound traffic, allowing Sysdig to download the latest external vulnerability feeds. 

  See [On-Prem Network and Port](/en/docs/administration/on-premises-deployments/architecture-system-requirements/system-requirements/#system-requirements) for more information. 

- **IP requirements:** For Sysdig to scan private repositories, your firewall must allow inbound requests from the [Sysdig IP addresses](/en/docs/administration/saas-regions-and-ip-ranges/#outbound-ip-addresses). 

## View Registry Scan Results

On the Registry page, you can search images and tags and assess the security posture of your images.

The **Registry** page displays scanning results from:

- The registry scans that leverage the registry scanner. By default, these scans are scheduled weekly; you can also trigger manual registry scans. 
- Instant image scanning using the **Scan Now** feature. This does not require deploying CLI tools into your environment. 


![](/image/vuln-scannerv1.png)

To view scan results:

1. Ensure that the registry scanner is [installed](/en/install-registry-scan/) and at least one scheduled scan job is completed, or  have one manual scan using **Scan Now** completed.  
2. Log in to Sysdig Secure and open  **Vulnerabilities** | **Findings** > **Registry**. You can:

   * Browse or search registries or repositories.
   * Search by image or tag.
   * Review detected vulnerabilities by severity and exploit status.
3. Select an image to access the tabs in the detail panel: **Overview**, **Vulnerabilities**, and **Content**.


| Tabs               | Preview           |
| ----------- | ------------------- |
| **Overview**: View **packages** and filter packages that are fixable. Click the individual cells to view the Vulnerabilities list. | ![img](https://docs.sysdig.com/image/reg-detail.png)  |
| **Vulnerabilities**: Use the expanded filters and clickable list of CVEs to view complete CVE details, including source data and fix information. The same security finding, such as a particular vulnerability, can be present in more than one rule violation table if it violates several rules. <br/><br>The example image shows the filtered result of vulnerabilties with  **Critical Severity**, **Has Fix**, and **Exploitable**. | ![img](https://docs.sysdig.com/image/reg-vuln.png)    |
| **Content**: You can view data organized by package view, with expanded filters and clickable CVE cells.  <br/>Use the **Content** tab to view cases, such as the software packages that are most dangerous. | ![img](https://docs.sysdig.com/image/reg-content.png) |


## Instant Image Scanning with Scan Now

You can instantly scan images in your registries and view results without deploying any CLI-based components. To do so:

- Integrate with a private container registry.
- Use the **Scan Now** button to initiate platform-based image scanning.

### Add a Container Registry

You can integrate private container registries and retrieve target images for scanning by configuring necessary registry credentials. Each of the registry types has unique input fields for the credentials required. For example, username/password for Docker Registry and JSON key for Google Container Registry. 

1. Log in to Sysdig Secure and select **Vulnerabilities > Registry** under **Findings**.

2. Select **Scan Now** > **Registry Credentials**.

3. Click **Add Registry** and enter:
   * **Registry Name**: A unique name to identify the registry.
   * **Path**: The path to the container registry, such as Docker Hub. The format is `<hostname>:<port>` or `<hostname>:<port>/<path>`. For example, `us-west1-docker.pkg.dev/my-registry/example-repo`
   
     If you are providing repository-specific credentials, provide the path to the repository.

     A registry might contain multiple namespaces in certain scenarios, each with distinct permissions. To encompass all the credentials under a single set, you can employ a partial path.
   
     Any image located within the partial path inside the registry will use the configured registry credentials.
   
   * **Type**: Select the type of Registry you are adding. Depending on your selection, additional parameters will appear, such as the **Access Key** and **Secret Key** for AWS ECR.
   
   * **Skip TLS Validation**: Toggle to disable secure validation.
   
4. Click **Add Registry Credential**. 

### Scan Now

1. Select **Vulnerabilities** > **Scan Now**.

   The **Scan Image** screen appears.

2. Enter your image reference to scan it directly from the registry.

3. Click **Scan Image**.

   You will encounter an error if you have not integrated the registry from which you are pulling the image.

4. To see the status of the scanning, select **Scan Now** > **Queue**.

5. Click on the row corresponding to an image to view the scan results.


## Next Steps

[Generate a registry report](/en/reporting-secure/)