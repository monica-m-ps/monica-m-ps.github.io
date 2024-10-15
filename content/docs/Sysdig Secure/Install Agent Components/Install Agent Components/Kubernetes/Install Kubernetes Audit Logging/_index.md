---
linkTitle: "Install Kubernetes Audit Logging"
title: "Install Kubernetes Audit Logging"
weight: "11"
no_list: true
aliases:
  - /en/docs/installation/admission-controller-installation/
  - /en/admission-controller/
  - /en/install-audit-logging/  
  - /en/docs/installation/sysdig-secure/install-agent-components/kubernetes/install-kubernetes-audit-logging/
  - /en/docs/installation/sysdig-secure/install-agent-components/kubernetes/install-kubernetes-audit-logging/
description: "Install and enable Kubernetes log integration so Sysdig Secure can use Kubernetes audit log data in the Events feed and Activity Audit."
---

The integration allows auditing of:

- Creation and destruction of pods, services, deployments, and daemon sets
- Creating/updating/removing config maps or secrets
- Attempts to subscribe to changes to any endpoint

There are two primary deployment methods:

- Using the admission-controller Helm chart
- Platform-based individual deployment

### Prerequisites

- Helm v3.8 or above
- [Installed Sysdig Components on Kubernetes using Helm](/en/install-kubernetes-secure)

- A [Sysdig Secure API token](/en/agent-access-key/)

  - Use a [custom role](/en/custom-role/) with the following [permissions for Sysdig Secure](/en/docs/administration/administration-settings/user-and-team-administration/manage-custom-roles/#sysdig-secure):

    Policies | Runtime Policies = `read`

    ![](/image/k8s_role_perm.png)

### Deployment

1. Recommended: Replace `helm install` with `helm upgrade --install`.

2. To enable Kubernetes audit logging in your existing Sysdig Secure Helm install command,  add the following flags:

   ```
    --set clusterShield.cluster_shield.features.audit.enabled=true 
   ```

3. For additional configuration options, including on-premises, using a proxy etc., see the [admission-controller readme](https://github.com/sysdiglabs/charts/tree/master/charts/admission-controller).

While using the `sysdig-deploy` chart, ensure that all the extra parameters set within the admission subchart are prefixed with `admissionController.`

### Verify the Installation

1. Install the Admission Controller on your Kubernetes cluster.

   This feature is enabled by default through the `features.k8sAuditDetections` value.

2. Check your current **Kubernetes Audit** policies in **Sysdig Secure** > **Policies** > **Threat Detection** | **Runtime Policies** as you will be triggering one of those to prove it's working correctly.

   We recommend  using "Create Privileged Pod" but you can choose any.

3. Activate just the installed component logs to have them at sight:

   ```yaml
   kubectl logs -f -n sysdig-admission-controller -l app.kubernetes.io/component=webhook
   ```

4. Trigger the following command to force an unwanted audit detection:

   ```yaml
   kubectl run nginx --image nginx --privileged
   ```

5. If you have activated logs, you should see something like this:

   ```
   {"level":"info","component":"console-notifier","message":"Pod started with privileged container (user=** pod=nginx ns=default images=nginx)"}
   ```

6. Confirm that the event is displayed in Events in the Sysdig Secure UI.

## AuditLog Options

If you do not want to use the Helm installation option, choose the steps appropriate to your environment, below.

These instructions assume:

- The Kubernetes cluster has NO audit configuration or logging in place.
- The steps add configuration only to route audit log messages to the Sysdig agent.
- Sysdig only supports the webhook backend, available in Kubernetes version 1.11.

There is a [beta script](#beta-script-to-automate-configuration-changes) automating many of these steps, which is suitable for *proof-of-concept*/*non-production* environments. In any case, we recommend reading the step-by-step instructions carefully before continuing.

The table below summarizes the tested options:

| Distro/Platform                                   | Version                                            | Uses Webhook | Uses Dynamic | Uses Other   |
| ------------------------------------------------- | -------------------------------------------------- | ------------ | ------------ | ------------ |
| [OpenShift](#openshift-42-43-legacy)              | 4.2, 4.3 (legacy)                                  |              | X            |              |
| [OpenShift](#openshift-42-43-legacy)              | 4.4+ not yet supported                             |              |              |              |
| [MiniShift](#minishift-311)                       | 3.11                                               | X            |              |              |
| [Kops](#kops)                                     | 1.15, 1.18, 1.20                                   | X            |              |              |
| [GKE (Google)](#gke)                              | 1.13                                               |              |              | X (bridge)   |
| [EKS (Amazon)](#eks)                              | eks.5/ Kubernetes 1.14 on AWS Cloud or AWS Outpost |              |              | X CloudWatch |
| [AKS (Azure)](#aks)                               | 1.15+                                              |              |              | X (bridge)   |
| [RKE (Rancher)](#rke-with-kubernetes-113)         | RKE v1.0.0/Kubernetes 1.13+                        | X            |              |              |
| [IKS (IBM)](#iks)                                 | 1.15                                               | X            |              |              |
| [Minikube](#minikube-and-kubeadm)                    | 1.11+                                              | X            |              |              |
| [Kubeadm](#minikube-and-kubeadm)                     | 1.11+                                              | X            |              |              |

### Prerequisites: Install Sysdig Agent and Apply the Agent Service

These instructions assume that the Sysdig agent has already been deployed to the Kubernetes cluster. See [Agent Installation](/en/install-components-secure) for details. When the agent(s) are installed, have the Sysdig agent service account, secret, configmap, and daemonset information on hand.

If the [sysdig-agent-service.yaml](https://github.com/draios/sysdig-cloud-scripts/blob/master/agent_deploy/kubernetes/sysdig-agent-service.yaml) was not explicitly deployed during agent installation, you need to apply it now:

```
kubectl apply -f https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-agent-service.yaml -n sysdig-agent  
```

Or with Helm

```
--set auditLog.enabled=true 
```

### MiniShift 3.11

Like OpenShift 3.11, Minishift 3.11 supports webhook backends, but the
way Minishift launches the Kubernetes API server is different.
Therefore, the command line arguments are somewhat different than in the
instructions above.

1. Copy the provided [audit-policy.yaml](https://github.com/draios/sysdig-cloud-scripts/blob/master/k8s_audit_config/audit-policy.yaml) file to the Minishift VM into the
   directory `/var/lib/minishift/base/kube-apiserver`/.

   (The file will be picked up by Minishift services running in
   containers because this directory is mounted into the kube API
   server container at `/etc/origin/master`.)

2. [Create a Webhook Configuration File](#create-a-webhook-configuration-file)
   and copy it to the Minishift VM into the
   directory `/var/lib/minishift/base/kube-apiserver/`.

3. Modify the master configuration by adding the following
   to `/var/lib/minishift/base/kube-apiserver/master-config.yaml`on
   the Minishift VM, merging/updating as required.

   **Note:**`master-config.yaml` also exists in other directories such
   as `/var/lib/minishift/base/openshift-apiserver` and
   `/var/lib/minishift/base/openshift-controller-manager/`.

   You should modify the one in `kube-apiserver`:

   ```yaml
   kubernetesMasterConfig:
     apiServerArguments:
     audit-log-maxbackup:
     - "1"
     audit-log-maxsize:
     - "10"
     audit-log-path:
     - /etc/origin/master/k8s_audit_events.log
     audit-policy-file:
     - /etc/origin/master/audit-policy.yaml
     audit-webhook-batch-max-wait:
     - 5s
     audit-webhook-config-file:
     - /etc/origin/master/webhook-config.yaml
     audit-webhook-mode:
     - batch
   ```

4. Restart the API server by running the following:

   ```yaml
   (For minishift)
   # minishift openshift restart
   ```

   Once restarted, the server will route Kubernetes audit events to the
   Sysdig agent service.

### OpenShift 4.2, 4.3 (Legacy)

By default, Openshift 4.2/4.3 enables Kubernetes API server logs and makes them available on each master node, at the path `/var/log/kube-apiserver/audit.log`. However, the API server is not  configured by default with the ability to create dynamic backends.

You must first enable the creation of dynamic backends by changing the API server configuration. You then create audit sinks to route audit events to the Sysdig agent.

1. Run the following to update the API server configuration:

   ```yaml
   oc patch kubeapiserver cluster --type=merge -p '{"spec":{"unsupportedConfigOverrides":{"apiServerArguments":{"audit-dynamic-configuration":["true"],"feature-gates":["DynamicAuditing=true"],"runtime-config":["auditregistration.k8s.io/v1alpha1=true"]}}}}'
   ```

2. Wait for the API server to restart with the updated configuration.

3. [Create a Dynamic Audit Sink](#create-a-dynamic-audit-sink).

   Once the dynamic audit sink is created, it will route Kubernetes
   audit events to the Sysdig agent service.

### Kops

You will modify the cluster configuration using `kops set`, update the
configuration using `kops update`, and then perform a rolling update
using `kops rolling-update`.

1. [Create a Webhook Configuration File](#create-a-webhook-configuration-file) and save it locally.

2. Get the current cluster configuration and save it to a file:

   ```yaml
   kops get cluster <your cluster name> -o yaml > cluster-current.yaml
   ```

3. To ensure that `webhook-config.yaml` is available on each master
   node at `/var/lib/k8s_audit`, and that the `kube-apiserver` process
   is run with the required arguments to enable the webhook backend,
   you will edit `cluster.yaml` to add/modify the
   `fileAssets` and `kubeAPIServer` sections as follows:

   ```yaml
   apiVersion: kops.k8s.io/v1alpha2
   kind: Cluster
   spec:
     ...
     fileAssets:
       - name: webhook-config
         path: /var/lib/k8s_audit/webhook-config.yaml
         roles: [Master]
         content: |
           <contents of webhook-config.yaml go here>
       - name: audit-policy
         path: /var/lib/k8s_audit/audit-policy.yaml
         roles: [Master]
         content: |
           <contents of audit-policy.yaml go here>
     ...
     kubeAPIServer:
       auditLogPath: /var/lib/k8s_audit/audit.log
       auditLogMaxBackups: 1
       auditLogMaxSize: 10
       auditWebhookBatchMaxWait: 5s
       auditPolicyFile: /var/lib/k8s_audit/audit-policy.yaml
       auditWebhookConfigFile: /var/lib/k8s_audit/webhook-config.yaml
     ...
   ```

   A simple way to do this using `yq` would be with the following
   script:

   ```yaml
   cat <<EOF > merge.yaml
   spec:
     fileAssets:
       - name: webhook-config
         path: /var/lib/k8s_audit/webhook-config.yaml
         roles: [Master]
         content: |
   $(cat webhook-config.yaml | sed -e 's/^/        /')
       - name: audit-policy
         path: /var/lib/k8s_audit/audit-policy.yaml
         roles: [Master]
         content: |
   $(cat audit-policy.yaml | sed -e 's/^/        /')
     kubeAPIServer:
       auditLogPath: /var/lib/k8s_audit/audit.log
       auditLogMaxBackups: 1
       auditLogMaxSize: 10
       auditWebhookBatchMaxWait: 5s
       auditPolicyFile: /var/lib/k8s_audit/audit-policy.yaml
       auditWebhookConfigFile: /var/lib/k8s_audit/webhook-config.yaml
   EOF
   
   yq m -a cluster-current.yaml merge.yaml > cluster.yaml
   ```

4. Configure Kops with the new cluster configuration:

   ```yaml
   kops replace -f cluster.yaml
   ```

5. Update the cluster configuration to prepare changes to the cluster:

   ```yaml
   kops update cluster <your cluster name> --yes
   ```

6. Perform a rolling update to redeploy the master nodes with the new
   files and API server configuration:

   ```yaml
   kops rolling-update cluster --yes
   ```

### GKE

{{% callout type="note" %}}

These instructions assume you have already created a cluster and
configured the `gcloud` and `kubectl` command-line programs to interact
with the cluster. Note the known limitations, below.

{{% /callout %}}

GKE already provides Kubernetes audit logs, but the logs are exposed
using Stackdriver and are in a different format than the native format
used by Kubernetes.

To simplify things, we have written a [bridge program](https://github.com/sysdiglabs/stackdriver-webhook-bridge/blob/master/stackdriver-webhook-bridge.yaml)that
reads audit logs from Stackdriver, reformats them to match the
Kubernetes-native format, and sends the logs to a configurable webhook
and to the Sysdig agent service.

1. Create a Google Cloud (not Kubernetes) service account and key that
   has the ability to read logs:

   ```yaml
   gcloud iam service-accounts create swb-logs-reader --description "Service account used by stackdriver-webhook-bridge" --display-name "stackdriver-webhook-bridge logs reader"
   gcloud projects add-iam-policy-binding <your gce project id> --member serviceAccount:swb-logs-reader@<your gce project id>.iam.gserviceaccount.com --role 'roles/logging.viewer'
   gcloud iam service-accounts keys create $PWD/swb-logs-reader-key.json --iam-account swb-logs-reader@<your gce project id>.iam.gserviceaccount.com
   ```

2. Create a Kubernetes secret containing the service account keys:

   ```yaml
   kubectl create secret generic stackdriver-webhook-bridge --from-file=key.json=$PWD/swb-logs-reader-key.json -n sysdig-agent
   ```

3. Deploy the bridge program to your cluster using the
   provided [stackdriver-webhook-bridge.yaml](https://github.com/sysdiglabs/stackdriver-webhook-bridge/blob/master/stackdriver-webhook-bridge.yaml)file:

   ```yaml
   kubectl apply -f stackdriver-webhook-bridge.yaml -n sysdig-agent
   ```

#### GKE Limitations

GKE uses a Kuberenetes audit policy that emits a more limited set of information than the one recommended by Sysdig. As a result, there are several limitations when retrieving Kubernetes audit information for the [Events feed](/en/events-secure/) and [Activity Audit](/en/activity-audit) features in Sysdig Secure.

**Request Object**

In particular, audit events for config maps in GKE generally do not contain a `requestObject` field that contains the object being created/modified.

**Pod exec does not Include command/container**

For many Kubernetes distributions, an audit event representing a
`pod exec` includes the command and specific container as arguments to
the requestURI. For example:

```
"requestURI":"/api/v1/namespaces/default/pods/nginx-deployment-7998647bdf-phvq7/exec?command=bash&container=nginx1&container=nginx1&stdin=true&stdout=true&tty=true
```

In GKE, the audit event is missing those request parameters.

**Implications for the Event Feed**

If the rule condition trigger includes a field that is not available in
the Kubernetes audit log provided by GKE, the rule will not trigger.

As a result, the following rule from [k8s\_audit\_rules.yaml](https://github.com/falcosecurity/falco/blob/dev/rules/k8s_audit_rules.yaml) will not trigger: `Create/Modify Configmap With Private Credentials`. (The contents of configmaps are not included in audit logs, so the contents
can not be examined for sensitive information.)

This will limit the information that can be displayed in the outputs of
rules. For example the  `command=%ka.uri.param[command]` output variable
in the `Attach/Exec Pod` rule will always return `N/A`.

**Implications for Activity Audit**

- `kubectl exec` elements will not be scoped to the cluster name; they
    will only be visible scoping by `entire infrastructure`"

- A `kubectl exec` item in Activity Audit will not display command or
    container information

- Drilling down into a `kubectl exec` will not provide the container
    activity as there is no information that allows Sysdig to correlate
    the `kubectl exec` action with an individual container.

### EKS

Amazon EKS does not provide webhooks for audit logs, but it allows audit
logs to be forwarded to [CloudWatch](https://aws.amazon.com/cloudwatch/).

To access CloudWatch logs from the Sysdig agent, proceed as follows:

1. Enable CloudWatch logs for your EKS cluster.

2. Allow access to CloudWatch from the worker nodes.

3. Add a new deployment that polls CloudWatch and forwards events to
    the Sysdig agent.

You can find an[example configuration](https://github.com/sysdiglabs/ekscloudwatch)that can be
implemented with the AWS UI, along with the code and the image for an
example audit log forwarder. (In a production system this would be
implemented as IaC scripts.)

Please note that CloudWatch is an additional AWS paid offering. In
addition, with this solution, all the pods running on the worker nodes
will be allowed to read CloudWatch logs through AWS APIs.

### AKS

These instructions assume you have already created a cluster and configured the Azure CLI and `kubectl` command-line programs to interact with the cluster.

Run the following script:

```yaml
curl -s https://raw.githubusercontent.com/sysdiglabs/aks-audit-log/master/install-aks-audit-log.sh | bash -s -- -g YOUR_RESOURCE_GROUP_NAME -c YOUR_AKS_CLUSTER_NAME
```

Resources will be created in the same resource group as your cluster:

- Storage Account, to coordinate event consumers

- Event Hubs, to receive audit log events

- Diagnostic setting in the cluster, to send audit log to Event Hubs

- Kubernetes deployment `aks-audit-log-forwarder`, to forward the log to Sysdig agent

You can verify that the audit logs are being forwarded by executing:

```yaml
kubectl get pods -n sysdig-agent
# take note of the pod name for aks-audit-log-forwarder
kubectl logs aks-audit-log-forwarder-XXXX -f
```

For additional information, optional parameters, and architecture details, see[the repository](https://github.com/sysdiglabs/aks-audit-log/blob/master/docs/readme-dev.md).

##### **To Uninstall**

Use the same parameters as for installation. The script will delete all created resources and configurations.

```yaml
curl -s https://raw.githubusercontent.com/sysdiglabs/aks-audit-log/master/uninstall-aks-audit-log.sh |  bash -s -- -g YOUR_RESOURCE_GROUP_NAME -c YOUR_AKS_CLUSTER_NAME
```

### RKE with Kubernetes 1.13+

These instructions were verified with RKE v1.0.0 and Kubernetes v1.16.3.
It should work with versions as old as Kubernetes v1.13.

Audit support is already enabled by default, but the audit policy must
be updated to provide additional granularity. These instructions enable
a webhook backend pointing to the agent's service. Dynamic audit
backends are not supported as there isn't a way to enable the audit
feature flag.

1. On each Kubernetes API Master Node, create the directory `/var/lib/k8s_audit`.

2. On each Kubernetes API master node, copy the provided [audit-policy.yaml](https://github.com/draios/sysdig-cloud-scripts/blob/master/k8s_audit_config/audit-policy.yaml) file
   to to the master node into the directory `/var/lib/k8s_audit`. (This
   directory will be mounted into the API server, giving it access to
   the audit/webhook files.)

3. [Create a Webhook Configuration File](#create-a-webhook-configuration-file) and copy it to each Kubernetes API master node, into the directory `/var/lib/k8s_audit`.

4. Modify your RKE cluster configuration `cluster.yml` to add `extra_args` and `extra_binds` sections to
   the `kube-api` section. Here's an example:

   ```yaml
   kube-api:
   ...
       extra_args:
         audit-policy-file: /var/lib/k8s_audit/audit-policy.yaml
         audit-webhook-config-file: /var/lib/k8s_audit/webhook-config.yaml
         audit-webhook-batch-max-wait: 5s
       extra_binds:
       - /var/lib/k8s_audit:/var/lib/k8s_audit
   ...
   ```

   This changes the command-line arguments for the API server to use an alternate audit policy and to use the webhook backend you created.

5. Restart the RKE cluster via `rke up`.

### IKS

IKS supports routing Kubernetes audit events to a single configurable webhook backend URL. It does not support dynamic audit sinks and does not support the ability to change the audit policy that controls which
Kubernetes audit events are sent.

The instructions below were adapted from the [IBM-provided documentation](https://cloud.ibm.com/docs/containers?topic=containers-health&amp;locale=en%5C043science) on how to integrate with Fluentd. It is expected that you are familiar with (or will review) the IKS tools for forwarding cluster and app logs described there.

**Limitation:** The Kubernetes default audit policy generally does not include events at the `Request` or `RequestResponse` levels, meaning that any rules that look in detail at the objects being created/modified
(e.g. rules using the `ka.req.*` and `ka.resp.*` fields) will not trigger. This includes the following rules:

- `Create Disallowed Pod`

- `Create Privileged Pod`

- `Create Sensitive Mount Pod`

- `Create HostNetwork Pod`

- `Pod Created in Kube Namespace`

- `Create NodePort Service`

- `Create/Modify Configmap With Private Credentials`

- `Attach to cluster-admin Role`

- `ClusterRole With Wildcard Created`

- `ClusterRole With Write Privileges Created`

- `ClusterRole With Pod Exec Created`

These instructions describe how to redirect from Fluentd to the Sysdig agent service.

1. Set the webhook backend URL to the IP address of the sysdig-agent service:

   ```yaml
   http://$(kubectl get service sysdig-agent -o=jsonpath={.spec.clusterIP} -n sysdig-agent):7765/k8s_audit
   ```

2. Verify that the webhook backend URL has been set:

   ```yaml
   ibmcloud ks cluster master audit-webhook get --cluster <cluster_name_or_ID>
   ```

3. Apply the webhook to your Kubernetes API server by refreshing the cluster master. It may take several minutes for the master to refresh.

   ```yaml
   ibmcloud ks cluster master refresh --cluster <cluster_name_or_ID>
   ```

### Minikube and Kubeadm

These instructions were verified using Minikube 1.19.0. Other Minikube versions should also work as long as they run Kubernetes versions 1.11. In all cases below, "the Minikube VM" refers to the VM created by
Minikube. In cases where you're using `--vm-driver=none`, this means the local machine.

1. Create the directory `/var/lib/k8s_audit` on the master node. (On Minikube, it must be on the Minikube VM.)

2. **For Kubernetes 1.11 to 1.18:** Copy the provided [audit-policy.yaml](https://github.com/draios/sysdig-cloud-scripts/blob/master/k8s_audit_config/audit-policy.yaml) file into the directory `/var/lib/k8s_audit`. (This directory will be mounted into the API server, giving it access to the audit/webhook files. On Minikube, it must be on the Minikube VM.)

   **For Kubernetes 1.19:** Use this [audit-policy.yaml](https://github.com/draios/sysdig-cloud-scripts/blob/master/k8s_audit_config/audit-policy-v2.yaml) file instead.

3. [Create a Webhook Configuration File](/en/install-audit-logging/ #create-a-webhook-configuration-file) and copy it to each Kubernetes API master node, into the
   directory `/var/lib/k8s_audit`.

4. Modify the Kubernetes API server manifest at `/etc/kubernetes/manifests/kube-apiserver.yaml`, adding the following command-line arguments:

   ```yaml
   --audit-log-path=/var/lib/k8s_audit/k8s_audit_events.log
   --audit-policy-file=/var/lib/k8s_audit/audit-policy.yaml
   --audit-log-maxbackup=1
   --audit-log-maxsize=10
   --audit-webhook-config-file=/var/lib/k8s_audit/webhook-config.yaml
   --audit-webhook-batch-max-wait=5s
   ```

   Command-line arguments are provided in the container spec as arguments to the program `/usr/local/bin/kube-apiserver`. The relevant section of the manifest will look like this:

   ```yaml
   spec:
     containers:
     - command:
       - kube-apiserver --allow-privileged=true --anonymous-auth=false
         --audit-log-path=/var/lib/k8s_audit/audit.log
         --audit-policy-file=/var/lib/k8s_audit/audit-policy.yaml
         --audit-log-maxbackup=1
         --audit-log-maxsize=10
         --audit-webhook-config-file=/var/lib/k8s_audit/webhook-config.yaml
         --audit-webhook-batch-max-wait=5s
         ...
   ```

5. Modify the Kubernetes API server manifest at `/etc/kubernetes/manifests/kube-apiserver.yaml`to add a mount of `/var/lib/k8s_audit` into the `kube-apiserver`container. The relevant sections look like this:

   ```yaml
   volumeMounts:
   - mountPath: /var/lib/k8s_audit/
     name: k8s-audit
     readOnly: true
        ...
   volumes:
   - hostPath:
     path: /var/lib/k8s_audit
     type: DirectoryOrCreate
     name: k8s-audit
       ...
   ```

6. Modifying the manifest restart the Kubernetes API server automatically. Once restarted, it will route Kubernetes audit events to the Sysdig agent's service.

### Prepare Webhook

Most of the platform-specific instructions will use one of these methods.

#### Create a Webhook Configuration File

Sysdig provides a templated resource file that sends audit events to an
IP associated with the Sysdig agent service, via port 7765.

It is "templated" in that the actual IP is defined in an environment
variable `AGENT_SERVICE_CLUSTERIP`, which can be plugged in using a
program like `envsubst`.

1. Download `webhook-config.yaml.in`.

2. Run the following to fill in the template file with the ClusterIP IP
   address associated with the `sysdig-agent service` you created,
   either when installing the agent or in the prereq step:

   ```yaml
   AGENT_SERVICE_CLUSTERIP=$(kubectl get service sysdig-agent -o=jsonpath={.spec.clusterIP} -n sysdig-agent) envsubst < webhook-config.yaml.in > webhook-config.yaml
   ```

   **Note:** Although service domain names like `sysdig-agent.sysdig-agent.svc.cluster.local` can not be resolved from the Kubernetes API server (they're typically run as pods but not really a part of the cluster), the ClusterIPs associated with those services are routable.

#### (BETA) Script to Automate Configuration Changes

As a convenience, Sysdig has created a
script: [enable-k8s-audit.sh](https://github.com/draios/sysdig-cloud-scripts/blob/master/k8s_audit_config/enable-k8s-audit.sh), which performs the necessary steps for enabling audit log support for
all Kubernetes distributions described above, **except EKS.**

You can run it via:`bash enable-k8s-audit.sh <distribution>` where `<distribution>` is one of the following:

- minishift-3.11
- gke
- iks
- rke-1.13
- kops
- minikube-1.13

Run from the `sysdig-cloud-scripts/k8s_audit_config`directory.

In some cases, it may prompt for the GCE project ID, IKS cluster name, etc..

For Minikube/Openshift-3.11/Minishift 3.11, it will use `ssh/scp`to copy files to and run scripts on the API master node. Otherwise, it should be fully automated.

### Test the Integration

To test that Kubernetes audit events are being properly passed to the
agent, you can do any of the following:

- Enable the **All K8s Object Modifications** policy and create a deployment, service, configmap, or namespace to see if the events are recorded and forwarded.

- Enable other policies, such as *Suspicious K8s Activity*,if and test them.

- You can use the [falco-event-generator](https://falco.org/docs/event-sources/sample-events/) Docker image to generate activity that maps to many of the default
  rules/policies provided in Sysdig Secure. You can run the image via a command line like the following:

  ```yaml
  docker run -v $HOME/.kube:/root/.kube -it falcosecurity/falco-event-generator k8s_audit
  ```

  This will create resources in a namespace `falco-event-generator`.

  See the native [Falco documentation](https://falco.org/docs/event-sources/kubernetes-audit/) for more information about this tool.

## Advanced Configuration for Helm Installation

### Proxy Usage

There are several configuration parameters for the proxy usage

- Two involved components `webhook.*` and `scanner.*`; reference to the first for communications to the Sysdig backend, while second communicates with the registry from where to pull the image to be scanned.
- configuration values `*.httpProxy`, `*.httpsProxy` and `*.noProxy`. Make sure to use at least `https` version for Sysdig Secure Backend.

If your Proxy is served with TLS:

- The URL for those `*.httpProxy` and `*.httpsProxy` must be `https://`
- If using a self-signed certificate you will need to also configure one of the following two options
  1. Set the `verifySSL=false` parameter
  2. Or set `*.ssl.ca.cert` for both components `webhook` and `scanner`

### On-Prem Settings

Sysdig On-Prem installations might use a TLS self-signed server certificate or one from an untrusted CA, so it requires an extra configuration.

#### Ignore TLS certificate verification

Use the following command to deploy in an on-prem and ignore the untrusted certificate using `verifySSL=false`:

```
$ helm upgrade --install sysdig-admission-controller sysdig/admission-controller \
      --create-namespace -n sysdig-admission-controller \
      --set clusterName=CLUSTER_NAME \
      --set sysdig.secureAPIToken=SECURE_API_TOKEN \
      --set verifySSL=false
```

#### Install with Custom CA

Deploy `admission-controller` with a custom CA using the following command.

```
$ helm upgrade --install sysdig-admission-controller sysdig/admission-controller \
      --create-namespace -n sysdig-admission-controller \
      --set clusterName=CLUSTER_NAME \
      --set sysdig.secureAPIToken=SECURE_API_TOKEN \
      --set webhook.ssl.ca.cert=YOUR_CA_CERT_AS_PEM_ENCODED \
      --set scanner.ssl.ca.cert=YOUR_CA_CERT_AS_PEM_ENCODED
```

The custom CA certificate is added to the trusted certificates store.

## Troubleshooting Helm Installation

### Alert Is Not Triggered for an Event with the `ka.verb=get` Condition

Though [Kubernetes Extensible Admission Controller webhook support it](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#matching-requests-rules), Sysdig Admission Controller does only handle `CREATE`, `UPDATE`, `DELETE` and `CONNECT` type of events. Also, beware Kubernetes `apiGroups` are scoped.

If required, you can use  the [legacy Sysdig Kubernetes Audit Log](/en/k8s-audit-log) which do support additional verbs.

### Switching to `debug` Verbose

If you have used helm to install, you can edit the helm `values.yaml` to set `webhook.logLevel=debug`
Alternatively, you can edit the webhook ConfigMap and add the `LOG_LEVEL=debug` key-value pair and restart the webhook.

```yaml
kubectl edit configmaps -n sysdig-admission-controller sysdig-admission-controller-webhook    
kubectl rollout restart deployment -n sysdig-admission-controller sysdig-admission-controller-webhook 
```

### Receiving Many TLS handshake Errors

You observe this  when `DEBUG` is enabled; however, Admission Controller works as expected. These calls are some non-Sysdig direct calls to the Admission Controller without TLS, which raises this informational log by the Go internal library.

### Admission Controller Is Slow and Cluster Is Not Scaling in a GKE Cluster with Private Network

GKE clusters run the Kubernetes API outside of the cluster. If Private Network is enabled, the Kubernetes API might be unable to reach the Admission Controller webhook that validates each API request, so eventually every API request times out and is processed, but the performance is impacted in the process. In such cases, you might observe the following error:

```
"Failed calling webhook, failing open audit.secure.sysdig.com: failed calling webhook "audit.secure.sysdig.com": Post "https://sysdig-ac-webhook.sysdig-agent.svc:443/k8s-audit?timeout=10s <https://sysdig-ac-webhook.sysdig-agent.svc/k8s-audit?timeout=10s>": context canceled" 
```

As specified in [GKE Private Cluster Webhook Timeouts](https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters#api_request_that_triggers_admission_webhook_timing_out), the default firewall configuration does not allow TCP connections for ports other than 443 and 10250. Admission Controller's webhook run on `5000 TCP port`, so you need to enable a new rule that allows the Control Plane's network to access it.

Follow the instructions in [GKE-Adding firewall rules to cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters#add_firewall_rules) to enable inbound connections to our webhook.

### Receiving  Permission Denied Error

Some users on old versions of GKE reported that the permissions to access serviceAccount token, mounted in the filesystem, was set to `0600` permissions, not allowing the pods to actually read from it. In such cases, you might see the following error;

```console
"error getting the cluster id from kubernetes: open /var/run/secrets/kubernetes.io/serviceaccount/token:permission denied
```

Changing the `securityContext.fsGroup` to the value `65534` on the pod is [recommended](https://github.com/kubernetes/kubernetes/issues/82573).  You can specify this in the helm chart with the following parameter:

```
--set webhook.podSecurityContext.fsGroup=65534 
```

### Receiving Readiness Probe Errors and Failing to Startup

```console
13m  Warning   FailedComputeMetricsReplicas   horizontalpodautoscaler/sysdig-admission-controller-webhook   invalid metrics (1 invalid out of 1), first error is: failed to get cpu utilization: unable to get metrics for resource cpu: unable to fetch metrics from resource metrics API: the server could not find the requested resource (get pods.metrics.k8s.io) 
```

[HorizontalAutoScaller](https://github.com/sysdiglabs/charts/blob/master/charts/admission-controller/templates/webhook/autoscaler.yaml) requires your kubernetes cluster to be able to use [metrics API](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#support-for-metrics-apis), which in some lightweight installations, such as minikube, must be enabled through a plugin.

For minikube, enable `metric-server` plugin as follows:

```
 minikube addons list | grep metrics-server $  minikube addons enable metrics-server 
```

### Receiving Unverified Certificate Error

Sysdig installation is made with an unverified certificate, such as self-signed, `SECURE_URL` being `https`, you will see the following error:

```
"x509: certificate signed by unknown authority"
```

To fix this issue, add `--set verifySSL=false` to your installation parameters.

### Kubernetes Audit Events Are not Generated for EKS Cluster Deployed Using Terraform

If Kubernetes Audit Events are not generated after installation of Admission Controller on EKS cluster that was deployed by using [EKS terraform module](https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest), additional node security group rule to allow traffic to port TCP/5000 might need to be added for the traffic originating from EKS cluster Control Plane towards webhook deployment of Admission Controller. See the example of the Terraform code snippet that needs to be added to the EKS module resource.

```
  # Additional node security group rules to allow access from the control plane to the Sysdig Admission Controller webhook port
  node_security_group_additional_rules = {
    ingress_allow_access_from_control_plane_to_sysdig_ac = {
      type                          = "ingress"
      protocol                      = "tcp"
      from_port                     = 5000
      to_port                       = 5000
      source_cluster_security_group = true
      description                   = "Allow access from control plane to webhook port of Sysdig Admission Controller"
    }
  }
```

## Next Steps

See results in the Sysdig Secure UI and work with the relevant policies and rules, as described in [Kubernetes Audit Logging](/en/k8s-audit-log).
