---
linkTitle: "Kubernetes Audit Logging"
title: "Kubernetes Audit Logging"
weight: "12"
aliases:
  - /en/kubernetes-audit-logging.html
  - /en/k8s-audit-log
description: "Integrate Kubernetes logs to see Kubernetes audit log data in Sysdig Secure's Events feed and Activity Audit."
---

This integration lets you audit:

- Creation and destruction of pods, services, deployments, daemon sets, and more.

- Creating, updating, and removing configmaps or secrets

- Attempts to subscribe to changes to any endpoint

## Prerequisite

- [Install Kubernetes Audit Logging](/en/install-audit-logging/) with Helm chart or a platform-specific installation procedure.  To enable this feature, ensure `features.k8sAuditDetections` is set to `true` (default value).

## Enable Kubernetes Audit on RKE

To enable Kubernetes Audit log on Rancher Kubernetes Engine (RKE), you must set `servies.kube-api.audit_log.enabled:` to `true`. 

## View Results in the UI

When Kubernetes audit logging is enabled, default audit policies are active and policy violations are visible in following locations:

- **[Events](/en/events-secure/) Feed**

  In the Sysdig Secure UI, select **Events**, and check for one of the Kubernetes Audit Policy names, such as *Sysdig K8s Notable Events*.

- **[Activity Audit]( /en/activity-audit)**

  In the Sysdig Secure UI, select **Investigate** > **Activity Audit** and filter for **Kubernetes**.

  ![](/image/aa_kube.png)

## Manage Relevant Policies and Rules

### Review Kubernetes Audit Policies

1. Log in to Sysdig Secure and select **Policies > Threat Detection > Runtime Policies**.

2. Open the **Select policy type** dropdown and choose **Kubernetes Audit**.

   ![](/image/kube_audit.png)

    The default managed policies and any additional custom policies are displayed.

3. You can:

   - Enable/disable existing policies
   - Create a custom Kubernetes audit policy
     For more information, see [Create policies](/en/docs/sysdig-secure/policies/manage-policies/#select-the-policy-type).

### Review Default Audit Logging Rules

The Kubernetes audit logging rules can be viewed in the Sysdig Policies
Rules Editor, found in the Policies module. To view the audit rules:

1. Log in to Sysdig Secure and select **Policies** > **Rules** > **Rules Editor**.

2. Open the drop-down for the default rules, and select
   `k8s_audit_rules.yaml`.

   <img src="/image/rules_k8.png" width="600" />

## Modify Default Audit Logging Rules

If you don't want to detect some resources within your Kubernetes cluser, you can create your custom rules.

To achieve this, you can change the `k8sAuditDetectionsRules` variable in the [values.yaml](https://github.com/sysdiglabs/charts/blob/admission-controller/charts/admission-controller/values.yaml) file. For example, if you want to filter out secrets from the admission controller you can use the following rules:

```yaml
- apiGroups:
  - ""
  apiVersions: [ "*" ]
  operations: [ "*" ]
  resources:
  - bindings
  - componentstatuses
  - configmaps
  - endpoints
  - events
  - limitranges
  - namespaces
  - nodes
  - persistentvolumeclaims
  - persistentvolumes
  - pods/*
  - podtemplates
  - replicationcontrollers
  - resourcequotas
  - serviceaccounts
  - services
  scope: "*"
- apiGroups:
  - apps
  - autoscaling
  - batch
  - networking.k8s.io
  - rbac.authorization.k8s.io
  - extensions
  apiVersions: [ "*" ]
  operations: [ "*" ]
  resources: [ "*/*" ]
  scope: "*"
```

See [Install Kubernetes Audit Logging](/en/install-audit-logging/ ) for more information on using the helm chart to apply the changes.