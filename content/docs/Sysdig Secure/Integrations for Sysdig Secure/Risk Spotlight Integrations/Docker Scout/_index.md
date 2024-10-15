---
linkTitle: "Docker Scout"
title: "Integrate Sysdig Risk Spotlight with Docker Scout"
weight: "11"
aliases:
  - /en/dockerscout
description: "Integrating Sysdig Secure into Docker Scout helps Docker Scout users prioritize vulnerabilities by indicating which images are active in runtime, and which packages are in use."
  
---

The APIs capture which images are running and where. In addition, they provide details about which packages are in use. This lets you view the Scout Dashboard and understand the potential impact to their deployed environments. Among the images in production, you can check which vulnerable packages are actually used, represent real risk, and therefore should be prioritized for remediation.

![](/image/diagram-dockerscout.png)

### Prerequisites

* **Have administrator rights to your Sysdig Secure SaaS** account and have the Sysdig Agent installed in the clusters you want to integrate.
* **Have a Docker account and have organizational owner rights** to enable the integration in the Docker Scout Dashboard.
* **Contact Sysdig Support** to explicitly enable Risk Spotlight in the backend for your Sysdig account.

### Integration

Ensure that your Sysdig Agents are properly configured. Contact your Sysdig representative to enable Risk Spotlight in the backend.


### Generate a Token for the Integration 

1. Select **Integrations > 3rd Party|Risk Spotlight Integration**. 
   The Spotlight Integration page appears with a list of existing tokens and their expiry dates. 

2. Click **+Add Token**.

   ![](/image/rsi2.png)

3. Fill in the attributes and click **Create Token**.

   * **Name:** Choose a name that indicates the integration with which the token is associated 
   * **Expiration:** Select an expiration date (`1/3/6 months`or `1 year`)

4. **Copy** the new token as it is displayed in the list. 

   Store the token in a safe place; it will not be visible or recoverable again. 

5. Continue with the [Docker Scout integration](https://docs.docker.com/scout/integrations/environment/sysdig/) guide.

To **Renew** a token at any time, click the **Renew** button, reset the expiry, and confirm. 

To **Delete** a token, click the **X** beside the token name and confirm. This action will sever the integration between Sysdig and the 3rd-party tool.

### Check Integration in Docker

Sysdig runtime insights information enriches Docker Scout images and findings, streamlining the prioritization process.

For more information about Docker Scout, see [environment monitoring](https://docs.docker.com/scout/integrations/environment/) and how **CLI** and **Web UI** results are enriched with this integration.

