---
linkTitle: "GCP"
title: "GCP"
weight: "20"
aliases:
  - /en/cloud-accounts-gcp
description: "This topic describes how to review details after you connect a Google Cloud Platform (GCP) projects to Sysdig Secure. You can add additional projects with the install wizard and validate the status of onboarded features."
---

## Access the Page

Log in to Sysdig Secure and select **Integrations > Cloud Accounts > GCP**  from the navigation bar. 

The GCP Cloud Accounts overview appears. 

![](/image/cloud_gcp3.png)

## Add GCP Account 

To connect a project, select **+ Add GCP Account**, and then follow the installation pop-up wizard.  

See also: [Connect Cloud Account | GCP](/en/gcp-secure) in the Installation documentation. 

## Review GCP Cloud Accounts 

The page lists:

- **Project:**  GCP **Project ID**, followed by a **Project Name**, if one was assigned.

- **Status:** The status of each GCP Project you connected. 

  - Possible values: `Connected`, `Pending`, `Partial Error`, `Error`, `Unknown`. For more on status values, see [Validate Account Connection](#validate-account-connection).

- **Last Checked:**  Time at which a feature was last validated (updated every 24 hours)

- **Org Domain:**  ID of the Organization to which the Project belongs, if applicable

- **Added On:** Date the Project was added to Sysdig Secure

### Validate Account Connection

There are two types integration status displayed, **Feature** level status, and **Project** level status. Status checks
are run approximately every 24 hours, with the first check occurring within an hour of enabling a feature.

{{% callout type="note" %}}

In the case of connection errors, if you remediate them the status will be updated on the page when the validation is run again, up to 24 hours later.

{{% /callout %}}

#### Feature Status

When you connect a cloud account to Sysdig Secure, you select the Sysdig features you would like to enable, such as Cloud Security Posture Management (CSPM), Cloud Infrastructure Entitlements Management (CIEM), Cloud Detection and Reponse (CDR) and Vulnerability Host Scanning. Features you've enabled will appear in the detail panel that opens when you select a row, where you can also see **Feature** connection status.

The possible **Feature** statuses are:

| Status Value   | Description                                                                            |
|----------------|----------------------------------------------------------------------------------------|
| Connected      | This feature was successfully onboarded and connected.                                 |
| Not Enabled    | This feature was not enabled during onboarding.                                        |
| Error          | There is an error in this feature connection.                                          |
| Pending        | This feature has been recently connected, and a validation will be run within an hour. |
| Unknown        | Sysdig cannot determine the current status of the feature.                             |

#### Subscription Status

The **Project** level status is shown in the main table, as well as at the top of the details panel. This status is an aggregate of the **Feature** statuses present in that Project.

The possible **Project** statuses are:

| Status Value  | Description      |
|---------------|---------|
| Connected     | All selected features were successfully onboarded and connected.                      |
| Partial Error | There is an error in at least one enabled feature.                                    |
| Error         | There are errors in all enabled features.                                             |
| Pending       | The Project has been recently connected, and a validation will be run within an hour. |
| Unknown       | Sysdig cannot determine the current status of the Project.                            |


## Detect all GCP Instances

Optional: Use a script to detect all the GCP instances of projects across your entire GCP organization. This information can help you to count all the places you may want to install an agent in tandem with the [Sysdig Agents](/en/sysdig-agents/) overview page.

Run this script to print out the number of instances in the project, and download a CSV file with the information:

```
echo "id,name,machine_type" > gcloud.csv
for PROJECT in $(gcloud projects list --format="value(projectId)")
do
  gcloud compute instances list --project $PROJECT --format="csv(
    id,
    name,
    machineType
  )" --quiet 2> /dev/null | grep -v "id,name" >> gcloud.csv
done
echo "Count of instances in all projects: $(cat gcloud.csv | tail -n +2| wc -l)"
```

The terminal entry displays the count similar to the following:

```
Count of instances in all projects:       21
```