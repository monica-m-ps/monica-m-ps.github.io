---
linkTitle: "Which Scanning Engine to Use"
title: "Which Scanning Engine to Use"
weight: "20"
aliases:
  - /en/new-scanning-engine.html
---

{{% callout type="warning" %}}

**End of Life Notice**: The Sysdig Legacy Scanning Engine will reach its End of Life (EOL) on **December 31st, 2024**. After this date, it will no longer be supported or maintained. Please upgrade to our [New Scanning Engine](/en/docs/sysdig-secure/vulnerabilities) before December 31st, 2024 to ensure continuous service and support. For assistance, contact our support team or your account representative.
{{% /callout %}}

As of , Sysdig offers both a **Legacy Scanner engine** and the newer **Vulnerability Management engine.**

### Which Engine is Enabled Now?

* If you have a top level menu entry labeled `Scanning`, then you are using the **[Legacy] Scanning engine**, and the relevant documentation is located [here](/en/docs/sysdig-secure/vulnerabilities/scanning/).

  ![img](/image/scan.png)

* Instead, if the top level menu entry is labeled `Vulnerabilities`, then you are using the **New Vulnerability Management engine**, and the relevant documentation is located [here](/en/docs/sysdig-secure/vulnerabilities/).

  ![img](/image/vuln.png)
  
* You can also enable both engines at the same time, if you need to take advantage of some legacy features (see below)

  ![](/image/vm_menu_both.png)

In general:

* **New Users** who installed Sysdig Secure for the first time after April 19, 2022 will be using Vulnerability Management by default
* **Earlier Users** who installed prior versions of Sysdig Secure will be using the Legacy Scanning engine and will need to enable the new Vulnerability Management to use it.

### How to Enable/Disable the Two Engines (UI)

1. Log in to Sysdig Secure as admin and choose `Settings > User Profile`.

2. Scroll down to the Sysdig Labs section to toggle on/off the New Vulnerabilities engine and/or the Legacy Scanning engine (with or without the legacy Admission Controller and Scheduled Reports).

   ![](/image/vm-labs.png)

{{% callout type="note" %}}
New Users (first install after April 19, 2022) must contact their Sysdig representative to have the legacy Labs toggles enabled in their Settings UI.
{{% /callout %}}

**Safe and transparent:**

* The toggles only alter the user interface and in no way impact the function or running of the engine itself.
* It is also safe to run the two engines side-by-side, (below)

### How to Enable/Disable the Two Engines (Backend)

The engine backends are enabled via options in the [Data Sources](/en/docs/sysdig-secure/integrations-for-sysdig-secure/data-sources/) page, under Install the Agent and Scan an Image.
