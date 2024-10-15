---
linkTitle: "Accepted Risk"
weight: "4"
aliases:
  - /en/vulnerability-accepted-risk
title: "Risk Acceptance for Vulnerabilities"
no_list: true
description: "You can choose to accept the risk of a detected vulnerability or asset. **Accept Risk** is available for both Runtime and Pipeline, and for specific CVEs or specified hosts or images."
---

## Prerequisites

Accept Risk requires Sysdig Secure SaaS to be installed with:

- `sysdig-deploy` Helm chart version 1.5.0+
- `cluster-shield` latest versions
- `sysdig-cli-scanner` version 1.13.0+

Because Accept Risk is applied to both pipeline and runtime vulnerability results impartially, the required versions of *both* components are required.

{{% callout type="important" %}}

If the minimum enablement requirements are not met, the **Accept Risk** button and panel will show in your interface, but will not activate. The created Acceptance will appear in **Pending** status for 20 minutes, then disappear as if you had never created it.  

{{% /callout %}}

## Check Your Versions

**Check `sysdig-deploy` Helm Chart:** Must be 1.5.0+

`helm list -n <namespace>` (default namespace is `sysdig-agent)`

Example:

```
$ helm list -n sysdig-agent
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
sysdig-agent    sysdig-agent    5               2022-11-11 17:57:54.109917081 +0100 CET deployed        sysdig-deploy-1.5.0
```

[Upgrade Helm Chart](/en/upgrade-agents).

**Check Cli Scanner**: must be 1.3.0+.

`./sysdig-cli-scanner --version`

[Upgrade CLI Scanner](/en/docs/sysdig-secure/install-agent-components/install-vulnerability-cli-scanner/)

## When to Use

When faced with a large number of reported vulnerabilities, organizations need to know which are the most relevant for their security posture. Sysdig already highlights critical vulnerability with a fix available, and vulnerabilities that occur in images actually in use.

An additional feature is the targeted ability to accept the risk of a vulnerability and not count it towards a policy violation, for example, when:

- An internal security team has analyzed the vulnerability and declared it a false positive
- The preconditions of the vulnerability don't apply
- Deployment in production is required and it is reasonable to postpone the fix

## What Types of Risk

You can accept risk for different entities:

- Individual CVE IDs
- Assets
  - Container images
  - Hosts

Accepting Risk in the context of vulnerability management applies an exception to the Vulnerability policy. Adding an ***accept** to a CVE doesn't make the CVE disappear. It still shows in the list, but voids the policy violation associated with that CVE.

When accepting risks it is important to:

- Be careful with the accept scope or context; overly broad exceptions can create false negatives.
  - Sysdig offers several scoping options for the accepts created.
- Remain aware of what is accepted so it doesn’t become a visibility gap.
- The Sysdig UI presents clear indications of what is accepted and why.
