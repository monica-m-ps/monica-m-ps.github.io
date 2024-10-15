---
linkTitle: "Snyk"
title: "Integrate Sysdig Risk Spotlight with Snyk"
weight: "10"
aliases:
  - /en/docs/sysdig-secure/integrations-for-sysdig-secure/risk-spotlight-integrations/integrate-effective-vulnerability-exposure-with-snyk/
  - /en/integrate-effective-vulnerability-exposure-with-snyk.html
  - /en/docs/sysdig-secure/integrate-effective-vulnerability-exposure-with-snyk/
  - /en/snyk
description: "[Snyk.io](https://snyk.io/) vulnerability management workflow can consume Runtime Insights information to filter and prioritize detected vulnerabilities, following a similar approach as [Risk Spotlight Integrations](/en/docs/sysdig-secure/integrations-for-sysdig-secure/risk-spotlight-integrations/)."
---


To integrate Sysdig Risk Spotlight information with Snyk vulnerability management workflows:

* Have an account and working license to use both products: [Snyk](https://snyk.io/product/container-vulnerability-management/), [Sysdig Secure](https://sysdig.com/products/secure/).
* Instrument the target runtime nodes using both products: [Snyk](https://snyk.io/product/container-vulnerability-management/), [Sysdig Secure](/en/installation#sysdig-agent).
* **Contact Sysdig Support** to explicitly enable Risk Spotlight in the backend for your Sysdig account.

Both Snyk and Sysdig instrumentation must be in place. Choose the installation path below that corresponds to the components installed on your infrastructure.  

## Installation Instructions

### Snyk Installed, Sysdig Not Installed

1. Note the namespace you are currently using to run the Snyk instrumentation. Default:  `snyk-monitor`.

2. Install Sysdig agent bundle [sysdig-deploy](https://charts.sysdig.com/charts/sysdig-deploy/) adding the `eveEnabled` parameter to the helm chart.

   Example:
   
   ```yaml
    helm install --namespace sysdig-agent sysdig-agent \
    ....other parameters...
     --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.deploy=true \
     --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.settings.eveEnabled=true \
     sysdig/sysdig-deploy
   
3. Make sure the Sysdig agent images and RuntimeScanner pods are running and healthy:

   ```kubectl -n sysdig-agent get po```
   ```
   NAME                                             READY   STATUS    RESTARTS   AGE
   sysdig-agent-8rmkt                               1/1     Running   0          24s
   sysdig-agent-hprw7                               1/1     Running   0          24s
   sysdig-agent-jrx2q                               1/1     Running   0          24s
   sysdig-agent-node-analyzer-5hltb                 4/4     Running   0          24s
   sysdig-agent-node-analyzer-b5ftm                 4/4     Running   0          24s
   sysdig-agent-node-analyzer-cd8rc                 4/4     Running   0          24s
   ```

4.  Update your Snyk monitor installation with Sysdig Risk Spotlight API Token and endpoint following Snyk [instructions](https://docs.snyk.io/products/snyk-container/image-scanning-library/kubernetes-workload-and-image-scanning/snyk-sysdig-integration).

### Sysdig Installed without EVE, Snyk Not Installed

If you already installed the Sysdig agent using the helm chart without enabling `eve` parameter, do the following:

1. Install Snyk instrumentation following its [documentation](https://docs.snyk.io/products/snyk-container/image-scanning-library/kubernetes-workload-and-image-scanning/snyk-sysdig-integration).

2. Upgrade the sysdig-deploy helm chart with the required eve settings:

   ```yaml
   helm upgrade sysdig-agent \
     --namespace sysdig-agent \
     --reuse-values \
     --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.deploy=true \
     --set nodeAnalyzer.nodeAnalyzer.runtimeScanner.settings.eveEnabled=true \
     sysdig/sysdig-deploy
   
### No Sysdig, No Snyk

1. Install the Sysdig agent bundle using the official [helm chart](https://charts.sysdig.com/charts/sysdig-deploy/), and including the steps and parameters from the first installation scenario.
2. Install Snyk instrumentation following its [documentation](https://docs.snyk.io/products/snyk-container/image-scanning-library/kubernetes-workload-and-image-scanning/snyk-sysdig-integration).

### Check Integration in Snyk UI

Check to confirm that runtime vulnerabilities are detected and prioritized in the Snyk UI: 
![](/image/snyk-interface.png)
