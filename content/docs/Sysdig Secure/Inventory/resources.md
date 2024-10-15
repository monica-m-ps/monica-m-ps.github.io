---
linkTitle: "Resources"
Title: "Resources"
weight: "1"
no_list: true
aliases:
  - /en/resources.html
  - /en/resources/
Description: "Use the **Resources** page to view cloud, Kubernetes, and container environment and explore findings for compliance and vulnerabilities."
---

Use the Inventory Resource page to: 

* View all **deployed** resources across your cloud environments and and **code** resources Infrastructure as Code (IaC) integrations.
* Find resources in your infrastructure that share properties, or belong to the same business unit.
* Take action on a posture violation or detected vulnerability by creating a Jira ticket, or accepting the risk.

## Enable Inventory Resources

| Resources                                                    | Configuration                                                | Required |
| :----------------------------------------------------------- | :----------------------------------------------------------- | -------- |
| Cloud resources (AWS, GCP, Azure)                            | Connect a cloud account.<br />See [Connect Cloud Accounts](/en/cloud-accounts-secure/). | Yes      |
| Kubernetes resources (Users, Roles, Groups, Hosts, Workloads…) | Install the Sysdig agent with KSPM enabled, using  `--set global.kspm.deploy=true \`. <br />See [Install Kubernetes](/en/install-kubernetes-secure). | Yes      |
| Container Images                                             | When installing the Sysdig agent for Kubernetes resources, above, also install the Runtime Image Scanner. This is included automatically when you install the agent using the Quick Start Wizard. <br />See [Install Kubernetes](/en/install-kubernetes-secure). | Yes      |
| Standalone Hosts (Linux, Docker)                             | Install the Posture Host Analyzer (non-Kubernetes) as a container. See [Install Posture Host Analyzer](/en/install-container-posture-host-analyzer) | No       |
| IaC Code                                                     | Check the [IaC Supportability Matrix](/en/iac-support).<br />- Set up a Git Integration.<br />- Add Git Sources.<br />See [Git Integrations](/en/git-integrations). | No       |
| Vulnerable cloud hosts                                       | Agent-based or Agentless [Vulnerability Host Scanning](/en/host-scanner/) | No       |
| Vulnerable packages running on Kubernetes Workloads          | Requires Risk Spotlight, which is auto-enabled from Sysdig agent v.12.15+.<br />See [Risk Spotlight](/en/risk-in-use). | No       |

## Navigate the Resources Page

To access and navigate Inventory Resources page:

1. Log in to Sysdig Secure and select **Inventory** > **Resources** from the left side bar.

   Inventory displays all resources from cloud accounts, Kubernetes data sources, and IaC Git resources connected to Sysdig, along with their findings for compliance, vulnerabilities, exposure.

   Data shown in the UI is refreshed every 24 hours when a compliance evaluation is run.

2. Use the featured and unified filter to find targeted resources. If an account has an alias, use the account alias to search for the account in Inventory instead of the Account ID. Filtering based on Account ID is not currently supported if the account has an alias. See [Filters and Queries](#filters-and-queries).

3. Use Resource Indicators: On a resource card, hover over the **Posture Policies Passing** gauge, the Runtime **Vulnerabilities** thermometer, and the Network Exposure icon to get high-level insights about your resource.

   ![](/image/inv_thermometer.png)

6. Click a resource card to review the resource Posture, Vulnerabilities and Configuration details.

## View Resource Details

Click on any resource to open the Details drawer. Here, you can use the different tabs to explore **Posture**, **Vulnerabilities**, **Packages** and **Configuration**. Available tabs will vary across different resources.

Use the Inventory search and filter bar to find resources with specific features. For example, you could filter all the resources with an Nginx package. Then, click any listed resource to see the packages tab, and find where the Nginx package resides.

### Use the Resource Posture Tab

1. Click a resource card to open details and access the **Posture** tab.

   ![](/image/inv_posture.png)

   The number of failed policies is highlighted next to the tab name.

2. Select a failed policy to see the relevant controls to be remediated. The controls are grouped by requirement within each policy.

    For more information, see [Evaluate and Remediate from Inventory](#evaluate-and-remediate-from-inventory).

### Use the Resource Vulnerabilities Tab

1. Click a resource card to open the details drawer and access the **Vulnerabilities** tab. It contains the latest runtime vulnerabilities scan results for that image or workload.

   ![](/image/inventory_vuln.png)

   The number of critical vulnerabilities is highlighted next to the tab name. If Risk Spotlight is enabled, the number is of critical vulnerabilities in use.

3. View detected vulnerabilities and filter on those with a **Fix**, an **Exploit**, or an **Accepted Risk**. Filtering on **In Use** is available for Workloads (see [Risk Spotlight](/en/risk-in-use)).  

   **NOTE:**

   * Runtime image scans include the **Image ID**. If the image was also scanned in Pipeline or Registry, then you can view it in the Pipeline or Registry Vulnerabilities interface using the 3-dot menu.

   * Cloud Host scans are performed against runtime packages.

   * Use the Image ID to view container images as a first-class citizen in Inventory or the Runtime Vulnerabilities interface from Workloads.

   * With Risk Spotlight enabled you can see which of your vulnerable packages are currently loaded in Runtime.

4. Click a CVE listing to open the full CVE details. Here you can see a description of the CVE, where else it has been reported, and whether a fix is available. You can also take action to [Accept the Risk](/en/docs/sysdig-secure/vulnerabilities/findings/runtime/#accept-risk-runtime) or [Create a Jira ticket](/en/docs/sysdig-secure/vulnerabilities/#remediate-with-jira).

     ![](/image/inv_cve_panel.png)

### Use the Resource Packages Tab

1. Click a resource card to open the resource details drawer and select the **Packages** tab. It shows what packages have been identified in your resources.

   ![](/image/inventory_package.png)

    In the **Packages** tab, you will see **Images**, a table of **Selected Image Packages**, and a filter. The number of packages with critical vulnerabilities is highlighted next to the tab name.

2. Use Filter and Search: Search by package name or path. Filter by type, severity, and whether the package is in use, has an exploit, or has a fix.

3. Review details to see whether there are vulnerability findings, identify package locations, and check whether any update or patch is necessary.

4. Apply suggested fixes.

### Use the Resource Configuration Tab

1. Click a resource card to open the details drawer and access the **Configuration** tab. It contains additional metadata and configuration details including the timestamp when the resource was last scanned.

   You can copy this or save it as a `.txt` file.

2. For Kubernetes hosts and clusters, you can search within the resource configuration by **Origin** or **Attribute**.

   ![](/image/inv_origin_attribute.png)

### Use the Network Exposure Tab

1. Click a resource card to open the details drawer and access the **Network Exposure** tab. 

  It provides data on how the resource is exposed. The number of exposed paths is highlighted beside the tab name.

2. Review highlighted Exposure path to see how a resource is exposed, internally or to the internet.

   ![](/image/inv_expose.png)

The following resources can have exposure tabs if they are exposed:

* Workloads.
* Hosts, such as Amazon Elastic Compute Cloud (Amazon EC2) Instances, Azure Virtual Machines, and Google Cloud Platform (GCP) Compute Instances.
* Storage, such as Azure Blob, AWS S3, and GCP Cloud Storage Buckets.

## Work with IaC Resources

If you have configured IaC scanning, you can view the resources supported by the Git-integrated scanner in the Inventory, such as YAML, Terraform, and Helm charts. The view of each code resource includes:

* resource metadata
* configuration details
* posture violations that can be remediated with automated workflows.

For more information, see [IaC Support](/en/iac-support).

### Search for IaC Resources

IaC resources from Git repository scans are labeled **code** in the Inventory UI, to distinguish them from **deployed** resources in your cloud environments.

1. Filter for `Resource Origin = Code`.

   In the resulting list, a visual tag `(< >)` on the logo to distinguish code resources.

   ![](/image/iac-list-inventory.png)

2. Other options: Filter by **Source Type**, **Location**, **Git Integration**, or **Repository**.

## View Vulnerable Resources

If you have installed the Runtime image scanner, you can view your container images in Inventory. For each resource, you can see:

* Resource metadata
* Runtime context of Package vulnerabilities
* Vulnerability details (has fix, has exploit) that support your remediation workflows

### Search for Container Images

Container images from Runtime scans are a new Resource Type in the Inventory UI.

* Filter for *Resource Type* `is` *Image*

Other options: Filter by *Platform, Image ID,* *Image Digest, Image Registry, Image Pullstrings* or *Image Tags*

### Search for Vulnerable Workloads

* Quickly discover workloads running container images with vulnerable packages.
* Filter for *Resource Type* `in` (*Deployment, DaemonSet, CronJob, Job*, etc) `AND` *Vulnerability* `in` `CVE-2005-2541`
* Other options: Filter by *Resource Type* **AND** *Package, Package Path, Vulnerability Severity*

### Search for Vulnerable Cloud Hosts

{{% callout type="note" %}}

Inventory currently supports AWS (*EC2 Instance*) and GCP (*Compute Instance*) Hosts. Azure VMs are out of scope.

{{% /callout %}}

Find cloud hosts running images with vulnerable packages.

* Filter for *Resource Type* **in** (*EC2 Instance, Compute Instance*) **AND** *Vulnerability* **in** `CVE-2023-0464`
* Other options: Filter for *Resource Type* **AND** *ARN, Resource ID*

## Filters and Queries

There are two filtering options in the Inventory UI: the Sysdig unified filter bar at the top, and the **Featured Filters** column on the left.

### Unified Filter

As in other parts of the Sysdig Secure interface, the unified filter bar allows you to build sophisticated queries using:

* Drop-down menu items and operators
* Clickable resource elements on the Inventory page

![](/image/filter_dropdown.png)

### Featured Filters

The Featured Filters panel, populated by Sysdig, displays your environment's most useful filter types. It lets you narrow down to your most prevalent and “at risk” resources and includes resource counting and risk indicators.

You can:

* Open and close the panel from the top-left arrow symbol.

* Click `In/!In` or `Yes/No` beside any featured filter item to include it in the query you are building in the unified filter.

* View by Risk indicators associated with Vulnerabilities: `Has Fix` and `Has Exploits`

  ![](/image/has_ex_has_fix2.png)

### Featured Queries

You can structure queries in the unified filter to solve common use cases as follows:

#### Find Resources by Name and Attributes

* I want to search for all S3 buckets in the EU regions
  * `Resource Type` **contains** `S3` AND `Region` **startsWith** `eu`
* I want to search for all Workloads of type host with names starting with prod
  * `Resource Type` **is** `host` AND `Name` **startsWith** `prod`
* I want to view the configuration of my GKE Worker nodes
  * `Node Type` **is** `Worker` AND `Kubernetes Distribution` **is** `gke`
* I want to search for all clusters running on OpenShift V4
  * `Resource Type` **is** `Cluster` AND `Kubernetes Distribution` **is** `ocp4`
* I want to search for all IaC resources
  * `Resource Origin` **is** `Code`

#### Find Resources Owned by Business Unit

* I want to search for all resources belonging to my PCI zones
  * `Zones` **startsWith** `PCI`
* I want to search for all resources labeled `app:retail`
  * `Labels` **in** `app:retail`
* I want to search for all resources within the `finance` namespace
  * `Namespace`  **is** `finance`
* I want to search for all resources belonging to my `docker` clusters
  * `Cluster` **contains** `docker`

#### Find your Environment Blind Spots

* I want to view all resources on which there are 0 policies applied
  * `Posture Applied Policy` **not exists**
* I want to view all resources that belong to 0 zones  
  * `Zones` **not exists**
* I want to view all resources that are not labeled
  * `Labels` **not exists**

#### Find Resources by Posture Details

* I want to view all resources on which policy X *is applied*
  * `Posture Applied Policy` **is** *(p1)*
  * `Posture Applied Policy` **in** *(p1, p2, p3)*
* I  want to view all resources on which `CIS Kubernetes V1.23 Benchmark` policy **is applied**
  * `Posture Applied Policy` **in** *CIS Kubernetes V1.23 Benchmark*
* I want to view all resources that *fail* ISO/IEC 27001 policy
  * `Posture Failed Policy` **in** `ISO/IEC 2700`
* I want to view all resources that are failing at least one policy
  * `Posture Failed Policy` `exists`
* I want to view all resources that are failing at least one control
  * `Posture Failed Contro`l `exists`
* I want to view all resources for which a risk has been accepted on a control
  * `Posture Accepted Risk` **yes**

#### Find Vulnerable Resources

* I want to view all Quay.io images that have the same Image ID.
  * `Resource Type` **is** *Image* **AND** `Platform` **is** *Quay.io* **AND** `Image ID` **is** *sha256:4fc533e8180ac3805582d3b2a9f8008d54d346211894d6131d92a82d17ee5458*
* I want to view all images that have Log4j packages and Apache Log4j Vulnerability.
  * `Resource Type` **is** *Image* **AND** `Package` **contains** *log4j* **AND** `Vulnerability` **in** *CVE-2021-44832*
* I want to view all images with critical vulnerabilities that have an exploit and a fix.
  * `Resource Type` **is** *Image* **AND** `Vulnerability Severity` **in** *Critical* **AND** `Has Exploit` **Yes** **AND** `Has Fix` **Yes**
* I want to view all workloads that have OpenSSL packages and OpenSSL Vulnerability.
  * `Resource Type` **is** *Deployment* **AND** `Package` **contains** *openssl* **AND** `Vulnerability` **in** *CVE-2023-0464*
* I want to view all workloads with critical vulnerabilities that have an exploit and a fix.
  * `Resource Type` **is** *DaemonSet* **AND** `Vulnerability Severity` **in** *Critical* **AND** `Has Exploit` **Yes** **AND** `Has Fix` **Yes**
* I want to view all workloads with In Use packages that belong to my HR team.
  * `Resource Type` **is** *StatefulSet* **AND** `In Use` **Yes** **AND** `Resource Labels` **in** *team:hr*

#### Find IaC Resources

* I want to find the code for the deployment of my mobile application's frontend.
  * `Resource Origin` **is** `Code` AND `Resource Type` **is** `Deployment` AND `Location` **contains** `mobile-app` AND `Name` **contains** `mobile-frontend`
* I want to see all Terraform code managed by the Marketing team
  * `Source Type` **is** `Terraform` AND `Labels` **in** `OwnedBy:team-marketing` AND `Repository` **is** `my-git-repo`

- I want to know which IaC of my EC2 Instances are failing a control
  * `Resource Origin` **is** `Code` AND `Resource Type` **is** `EC2 Instance` AND `Posture Failed Control` **exists**
* I want to see all IaC repos not being evaluated by policies
  * `Git Integration` **is** `my-iac-repos` AND `Posture Applied Policy` **not exists**

#### View a Resource’s Applied Configuration

* I want to view the bucket policy for `bucket` `123`
  * `Resource Type` **contains** `bucket` AND `Name` **contains** `123` + click on resource card to scroll through configuration details
* I want to view the default `runAsUser` applied configuration for workload `abc`
  * `Name` **is** `abc` + click on resource card to scroll through configuration details

## Evaluate and Remediate from Inventory

You can evaluate and remediate posture violations for your deployed and code (IaC) resources from the Inventory interface.

The steps are the [same as in the Compliance module](/en/docs/sysdig-secure/posture/compliance/#evaluate-and-remediate), with the following modifications:

* For IaC resources, you cannot remediate manually or select **Accept Risk**.

For certain controls, you can open a pull request to remediate a code resource.

![](/image/inventory-remed2.png)

## Use the Inventory API

Query the Secure API to get a list of multiple inventory resources or retrieve a single one.

* For details, see the [Inventory entries]() in the API documentation.

* For API doc links for additional regions or steps to access them from within the Sysdig Secure UI, see the [Developer Tools](/en/docs/developer-tools/) overview.

{{% callout type="note" %}}

At this time, only resource metadata and posture data are in scope. Vulnerabilities, package, and exposure data are currently not available.

{{% /callout %}}

## Inventory Data Dictionary

You can construct searches/filters by attribute name in the Inventory *unified filter*.

| Filter Operators                                             |
| ------------------------------------------------------------ |
| is (=) <br />is not (!=) <br />in (in) <br />not in (!in) <br />yes (Y)<br />no (N) <br />startsWith (^) <br />contains (%)  (wildcards not supported)<br />exists/not exists |

| Attribute Name                    | Attribute Definition                                         |
| --------------------------------- | ------------------------------------------------------------ |
| Account                           | The container for your AWS resources. You create and manage your AWS resources in an AWS account. If an account has an alias, use the alias to search for the account in Inventory instead of the Account ID. |
| Architecture                      | The CPU architecture for which your container image is built |
| ARN                               | The unique identifier of your AWS cloud resource             |
| Attribute*                        | The attributes defined within the configuration of your Kubernetes host or cluster<br />* Only filtered from within a Kubernetes host or cluster resource configuration. |
| Base OS                           | Base operating system of your container image                |
| Category                          | Classification or grouping of your resources and services based on their functionalities, characteristics, and use cases. |
| Cluster                           | Name of your Kubernetes cluster                              |
| Containers                        | The container name(s) of your workload(s)                    |
| Created                           | Date your container image was created                        |
| CVSS                              | The CVSS score (0.0 - 10.0) of the CVE                       |
| External DNS                      | The DNS name of your Kubernetes host's node that will resolve into an address with external address characteristics |
| Git Integration                   | Name of the Git integration. See [IaC Scanning/Git Integrations](/en/git-iac-scanning) |
| Git Repository                    | Name of the repository, example: *IaC_Demo*                  |
| Has Exploit                       | If the vulnerability has a known exploit                     |
| Has Fix                           | If the vulnerability has a known fix                         |
| Host Image ID                     | The unique identifier associated with a custom or public image used to create and launch your virtual machine instances.  Examples:<br />AWS: *ImageID* (ex: ami-0123456789abcdef0)<br />GCP: *sourceImage* or *sourceDisk* (ex: projects/canonical-cloud/global/images/family/ubuntu-2004-lts) |
| Host OS                           | Operating System of your Kubernetes host                     |
| Image Digest                      | Hash value computed from the content of your container image to verify its integrity (ex: sha256:0278c0...) |
| Image ID                          | Unique identifier assigned to your container image (ex: sha256:062ab3...) |
| Image OS                          | The operating system environment encapsulated within your container image |
| Image Pullstrings                 | The path where your container image was pulled from in the registry |
| Image Registry                    | Storage system where your container is hosted and managed    |
| Image Tags                        | Labels or identifiers associated with your container images (ex: latest, v1.4) |
| In Use                            | If the package is running                                    |
| Kubernetes Distribution           | GKE, EKS, AKS, Rancher, Vanilla, OpenShift v4, IKS, MKE      |
| Location                          | Path to the file or module directory in the repository (IaC resources):<br />- Kubernetes manifest: path to the `YAML` file <br />- Helm charts: `Helm` chart folder  <br />- Kustomize: path to the manifests folder (folder containing the base `kustomization.yaml` file) <br />- Terraform: path to the `.tf` module folder |
| Namespace                         | Kubernetes cluster namespace                                 |
| Node Type                         | Master or Worker node of your Kubernetes host                |
| Organization                      | Root node of your managed cloud resources hierarchy          |
| Origin*                           | The origin of your Kubernetes host's or your cluster's configuration (Docker, Linux, Kubernetes, etc.)<br />* Only filtered from within a Kubernetes host’s resource configuration.. |
| OS Image                          | Name and version of your host's Operating System             |
| Package                           | The name of a package                                        |
| Package Path                      | The file system location or directory path where your software package is stored or installed |
| Package Type                      | The format or structure used to package and distribute your software (ex: Java, OS) |
| Platform                          | AWS, Azure, GCP, Kubernetes, or Linux                        |
| Posture Accepted Risk             | Whether or not a [risk has been accepted](/en/docs/sysdig-secure/posture/compliance/#option-accept-risk) for a resource's control |
| Posture Applied Policy            | Name(s) of the policies applied to the resource              |
| Posture Failed Control Severity   | High, medium, or low                                         |
| Posture Failed Control            | Name(s) of the failed control(s) applied to the resource     |
| Posture Failed Policy             | Name(s) of the failed policy(ies) applied to the resource    |
| Posture Failed Requirement        | Name(s) of the failed requirement(s) applied to the resource |
| Posture Passed Policy             | Name of the passed policy applied to the resource            |
| Project                           | The container for your Google Cloud resources. You create and manage your GCP resources in a GCP project |
| Exposed                           | If the resource is publicly or ingress exposed               |
| Region                            | Region of the world where your managed cloud resource is deployed (such as us-east, eu-west, asia-northeast) |
| Resource ID                       | The unique identifier of your Google or Azure cloud resource |
| Resource Labels                   | Labels are key/value pairs, such as `team:research`, that you have applied to your resource from Kubernetes or Managed Cloud platforms (AWS, GCP, Azure). For Kubernetes, the Labels field includes all the labels for the resource, including nested labels. For example, a search on a label that exists on a container will return the workload that contains the container. |
| Resource Name                     | Name of your resource                                        |
| Resource Origin                   | - *Code:* IaC resources from Git integrations <br />- *Deployed:* Runtime resources |
| Resource Type                     | For Kubernetes, can be a *Workload*, *Service Account*, *Role*, *Cluster Role*, *Host*, *User,* *Cluster, or* *Group*. <br />For managed clouds, it can be a *Resource (S3 bucket, Deployment, DaemonSet...)* or an *Identity (Access Key, User, Policy...)* |
| Source Type                       | Possible values for IaC source:  *YAML, Helm, Kustomize, Terraform* |
| Subscription                      | The container for your Azure resources. You create and manage your Azure resources in an Azure subscription |
| Version/OpenShift Cluster Version | The version of your OpenShift cluster                        |
| Vulnerability                     | The CVE identified on your vulnerable package                |
| Vulnerability Severity            | The severity of the CVE                                      |
| Zones                             | A business group of resources, defined by a collection of scopes of various resource types (for example: “Dev” - all my development resources) |

#### Additional Attributes

Attributes that appear in the interface but cannot be searched/filtered on include:

| Attribute Name | Attribute Definition                                         |
| -------------- | ------------------------------------------------------------ |
| Last Seen      | Last date when the resource was evaluated                    |
| Value*         | The value of your Kubernetes host’s or your cluster’s attribute. <br />* Only for Kubernetes hosts and clusters |

![](/image/host-attribute.png)
