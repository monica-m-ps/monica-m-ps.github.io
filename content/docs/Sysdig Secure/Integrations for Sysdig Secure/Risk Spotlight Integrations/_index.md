---
linkTitle: "Risk Spotlight Integrations"
title: "Risk Spotlight Integrations"
weight: "12"
aliases:
  - /en/risk-spotlight-integrations.html
  - /en/risk-spotlight-integrations
description: "Sysdig has a simplified way to integrate third-party tools with Risk Spotlight and In Use features."
---

## Integrate with External Platforms

There are two integration models: in-cluster (for Snyk) and API-based (all others). The installation instructions for each are different. 

* **For Snyk:** See the [Snyk integration page](/en/snyk).

* **For all others:** Use the following steps:

### Generate a Token for the Integration 

1. From the left navigation bar, select **Integrations > Risk Spotlight Integration**. 

   The Spotlight Integration page appears, with a list of existing tokens and their expiry dates. 

2. Click **+Add Token**.

   ![](/image/rsi2.png)

3. Fill in the attributes and click **Create Token**.

   * **Name:** Choose a name that indicates the integration with which the token is associated.
   * **Expiration:** Select an expiration date (`1/3/6 months`; `1 year`).

4. **Copy** the new token as it is displayed in the list. 

   Store the token in a safe place; it will not be visible or recoverable again. 

To **Renew** a token at any time, click the `Renew` button, reset the expiry, and confirm. 

To **Delete** a token, click the `X` beside the token name and confirm. This action will sever the integration between Sysdig and the 3rd-party tool. 

### Follow the Platform-Specific Integration Steps

**Current integrations include:** 

**[Docker Scout](/en/dockerscout)**

* Check the prerequisites.
* Follow the third-party integration guide provided, adding the Sysdig token as prompted.
* Verify the integration in the third-party UI.