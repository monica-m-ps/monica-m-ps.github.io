---
linkTitle: "Troubleshoot"
title: "Troubleshoot"
weight: "10"
no_list: true
aliases:
  - /en/azure-secure-config/
  - /en/docs/installation/sysdig-secure/connect-cloud-accounts/azure/troubleshoot
description: "This page describes troubleshooting for Azure cloud account setup."
---

## Insufficient Permissions on Subscription

This error might occur if your current Azure authentication session does not have the required permissions to create resources in the specified subscription.

Solution:
Ensure you are authenticated to Azure using a Non-Guest user with the Contributor or Owner role on the target subscription.

```
Error: Error Creating/Updating Lighthouse Definition "dd9be15b-0ee9-7daf-b942-5e173dae13fb" (Scope "/subscriptions/***"): managedservices.RegistrationDefinitionsClient#CreateOrUpdate: Failure sending request: StatusCode=0 -- Original Error: Code="InsufficientPrivilegesForManagedServiceResource" Message="The requested user doesn't have sufficient privileges to perform the operation."
       
     with module.cloudvision_example_existing_resource_group.module.cloud_bench.module.trust_relationship["***"].azurerm_lighthouse_definition.lighthouse_definition,
         on ../../../modules/services/cloud-bench/trust_relationship/main.tf line 28, in resource "azurerm_lighthouse_definition" "lighthouse_definition":
         28: resource "azurerm_lighthouse_definition" "lighthouse_definition" {
```

## Conflicting Resources

This error might occur if the specified Azure Subscription has already been onboarded to Sysdig

Solution:
You can import the resource into Terraform using the `terraform import` command. This will bring the resource under management in the current Terraform workspace.

```
Error: A resource with the ID "/subscriptions/***/resourceGroups/sfc-resourcegroup" already exists - to be managed via Terraform this resource needs to be imported into the State. For details, see the resource documentation for `azurerm_resource_group`.
       
         with module.cloudvision_example_existing_resource_group.module.infrastructure_eventhub.azurerm_resource_group.rg[0],
         on ../../../modules/infrastructure/eventhub/main.tf line 6, in resource "azurerm_resource_group" "rg":
          6: resource "azurerm_resource_group" "rg" {
```

## Missing Microsoft.ManagedServices Namespace Registration

This error occurs if the specified Azure Subscription is not registered in the 'Microsoft.ManagedServices' namespace.

Solution:
You can register a Resource Provider by following the [Microsoft Documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types#:~:text=Azure%20portal-,Register%20resource%20provider,-To%20see%20all).

``` terraform
Error: creating/updating Lighthouse Assignment: (Lighthouse Definition ID "***" / Scope "/subscriptions/***"): managedservices.RegistrationDefinitionsClient#CreateOrUpdate: Failure sending request: StatusCode=409 -- Original Error: Code="MissingSubscriptionRegistration" Message="The managed services resource provider not allowed to access the subscription '***'. The subscription must be registered to use namespace 'Microsoft.ManagedServices'. Please see https://aka.ms/rp-not-register-error for details on how to register subscriptions." Details=[{"code":"AuthorizationFailed","message":"The client '***' with object id '***' does not have authorization to perform action 'Microsoft.Resources/subscriptions/read' over scope '/subscriptions/***' or the scope is invalid. If access was recently granted, please refresh your credentials."}]

  with module.subscriptions-sysdig-sfc-agentless["***"].module.trust_relationship["***"].azurerm_lighthouse_definition.lighthouse_definition,
  on .terraform\modules\subscriptions-sysdig-sfc-agentless\modules\services\cloud-bench\trust_relationship\main.tf line 24, in resource "azurerm_lighthouse_definition" "lighthouse_definition":
  24: resource "azurerm_lighthouse_definition" "lighthouse_definition" {
```

## Offboard Azure Subscriptions|Groups|Tenants using Terraform

**Problem: Cannot use Terraform Destroy to offboard**

Azure Service Principals are created at the Tenant level. Therefore, onboarded subscriptions share and use an existing service principal that is linked to the Sysdig application in the same tenant. 

Once created, this service principal cannot be deleted via the Terraform module. This safeguards against unintended deletes if the service principal is in use.

As a result, trying to offboard using Terraform `destroy` results in the following: *Error: Instance cannot be destroyed.*

![](/image/TF_SP_prevent_destroy.png)

**Solution:**

To resolve this problem,  remove the `service_principal` resource from your Terraform state, so that Terraform will no longer control it. 

```
1. terraform state list 
2. terraform state rm module.<module-name>.azuread_service_principal.<sysdig-sp-resource-name>
3. terraform destroy
```