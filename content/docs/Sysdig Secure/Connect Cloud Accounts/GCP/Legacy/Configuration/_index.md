---
linkTitle: "Configuration"
title: "Configuration"
weight: "11"
no_list: true
aliases:
  - /en/gcp-secure-config
  - /en/gcp-secureconfiguration/
description: "Check configuration and troubleshooting steps for GCP cloud account setup."

---
## Customize the Install

Both the Single Account and Organizational Account code examples are
configured with sensible defaults for the underlying inputs. But if
desired, you can edit the region, module enablement, and other Inputs.
See details for:

- [Single Project setup inputs options](https://github.com/sysdiglabs/terraform-google-secure-for-cloud/tree/master/examples/single-project#inputs)

- [Organization setup input options](https://github.com/sysdiglabs/terraform-google-secure-for-cloud/tree/master/examples/organization#inputs)

## Troubleshooting

> Find more troubleshooting options on the [Secure for Cloud - Terraform GCP module source repository](https://github.com/sysdiglabs/terraform-google-secure-for-cloud#troubleshooting)

#### 1. Insufficient Permissions on Project

This error may occur if your current GCP authentication session does not have the required permissions to access the specified project.

Solution:
Ensure you are authenticated to GCP using a user or service account with the required permissions.

![](/image/case1.png)

#### 2. Insufficient Permissions to Create Resource

This error may occur if your current GCP authentication session does not have the required permissions to create certain resources.

Solution:
Ensure you are authenticated to GCP using a user or service account with the required permissions.

![](/image/case2.png)

If you have sufficient permissions but still get this kind of error, try to authenticate `gcloud` using:

`$ gcloud auth application-default login`

```
gcloud auth application-default set-quota-project your_project_id
```

#### 3. Conflicting Resources

This error may occur if the specified GCP project has already been onboarded to Sysdig.

Solution:
The cloud account can be imported into terraform by running

`terraform import module.single account.module.cloud_bench.sysdig_secure_cloud_account.cloud_account PROJECT_ID` , where `PROJECT_ID` is the numerical ID of the project (not the project name).

![](/image/case3.png)

#### 4. Workload Identity Federation pool already exists

This error may occur if a Workload Identity Federation Pool or Pool Provider has previously been created, and then deleted, either via the GCP console or with `terraform destroy`. When a delete request for these resources is sent to GCP, they are not completely deleted, but marked as "deleted", and remain for 30 days. These "deleted" resources will block creation of a new resource with the same name.

![](/image/case4.png)

Solution:
The "deleted" pools must be restored using the GCP console, and then imported into terraform

```
# re-activate pool and provider
$ gcloud iam workload-identity-pools undelete sysdigcloud  --location=global
$ gcloud iam workload-identity-pools providers undelete sysdigcloud --workload-identity-pool="sysdigcloud" --location=global

# import to terraform state
# for this you have to adapt the import resource to your specific usage
# ex.: for single-project, input your project-id
$ terraform import 'module.secure-for-cloud_example_single-project.module.cloud_bench[0].module.trust_relationship["<PROJECT_ID>"].google_iam_workload_identity_pool.pool' <PROJECT_ID>/sysdigcloud
$ terraform import 'module.secure-for-cloud_example_single-project.module.cloud_bench[0].module.trust_relationship["<PROJECT_ID>"].google_iam_workload_identity_pool_provider.pool_provider' <PROJECT_ID>/sysdigcloud/sysdigcloud

# ex.: for organization example you should change its reference too, per project
$ terraform import 'module.secure-for-cloud_example_organization.module.cloud_bench[0].module.trust_relationship["<PROJECT_ID>"].google_iam_workload_identity_pool.pool' <PROJECT_ID>/sysdigcloud
$ terraform import 'module.secure-for-cloud_example_organization.module.cloud_bench[0].module.trust_relationship["<PROJECT_ID>"].google_iam_workload_identity_pool_provider.pool_provider' <PROJECT_ID>/sysdigcloud/sysdigcloud
```

The import resource to use, is the one pointed out in your terraform plan/apply error messsage

 ```
 # for
Error: Error creating WorkloadIdentityPool: googleapi: Error 409: Requested entity already exists
 with module.secure-for-cloud_example_organization.module.cloud_bench[0].module.trust_relationship["org-child-project-1"].google_iam_workload_identity_pool.pool,
 on .... in resource "google_iam_workload_identity_pool" "pool":
 resource "google_iam_workload_identity_pool" "pool" {
 
# use
' module.secure-for-cloud_example_organization.module.cloud_bench[0].module.trust_relationship["org-child-project-1"].google_iam_workload_identity_pool.pool' as your import resource

# such as
$ terraform import 'module.secure-for-cloud_example_organization.module.cloud_bench[0].module.trust_relationship["org-child-project-1"].google_iam_workload_identity_pool.pool' 'org-child-project-1/sysdigcloud'

 ```

#### 5. Received an email from Google Cloud Platform citing a Configuration Error

Email contains error codes such as:

```
Error Code: topic_not_found
or
Error Code: topic_permission_denied
```

Cause: The resources Sysdig deployed with Terraform will eventually be consistent, but it could happen that some pre-required resources are created but not ready yet.

Solution: This is a known issue that will only take place within first minutes of the deployment. Eventually, resource health checks will pass and modules will work as expected.

![](/image/gcp-troubleshoot-email-error.png)
