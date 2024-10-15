---
linkTitle: "Sysdig Monitor and Sysdig Secure"
title: "Sysdig Monitor and Sysdig Secure"
weight: "20"
aliases:
  - /en/install-monitor-secure
  - /en/docs/installation/sysdig-monitor-and-sysdig-secure/
description: "This topic helps you connect your cloud accounts and install the Sysdig Agent when using Sysdig Monitor and Sysdig Secure together."
---

## Sysdig Agent

**To use Sysdig Monitor + Secure (Sysdig Platform), follow the [Sysdig Secure installation process](/en/install-secure).** This installs the Sysdig Agent, which is used by both products, and delivers the monitoring and troubleshooting capabilities of Sysdig Monitor along with the runtime security, compliance, and vulnerability management features of Sysdig Secure.

For Monitor-specific features and environments, see:

- [Set up Prometheus Remote Write](/en/prometheus-remote-write)

- [Set up integrations for Sysdig Monitor](/en/integrations-for-sysdig-monitor)

{{% callout type="note" %}}

If you have a Platform license (Sysdig Monitor and Sysdig Secure), no configuration is required to enable [Agent Modes](/en/configure-agent-modes). All the Sysdig Platform features are enabled by default. If you are opting for a subset of features, determine the Agent Mode corresponding to your requirement and proceed to the corresponding installation section.

{{% /callout %}}  

## Cloud Accounts

Connecting to Cloud Accounts is different in Sysdig Monitor and Sysdig Secure. If you are using both Monitor and Secure and want to connect a cloud account, you need to do it separately for each product. While Sysdig Monitor collects cloud metrics and provides with monitoring dashboards and alerts, Sysdig Secure creates resources to  detect and respond to runtime threats, and continuously manage cloud configurations, permissions, and compliance. To learn more about cloud integrations, see:

[Sysdig Monitor](/en/cloud-metrics)

[Sysdig Secure](/en/cloud-accounts-secure)
