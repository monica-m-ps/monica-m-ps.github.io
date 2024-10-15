---
title: "Support Policy for Sysdig Agent"
linkTitle: "Support Policy for Sysdig Agent"
weight: "17"
no_list: true
aliases:
  - /en/support-policy
  - /en/docs/installation/support-policy/
description: "Sysdig is committed to providing reliable and efficient support for the Sysdig Agent. This section outlines the support policy for the Sysdig Agent, including our version support and end-of-support policies."  
---
## Monthly Release Cadence

Sysdig releases updates to the Sysdig Agent on a monthly schedule. This approach ensures a stable and predictable release schedule for our users. Each monthly release might include enhancements, defect fixes, performance improvements, and new features.

This monthly cadence allows for a consistent development and testing cycle that strikes a balance between innovation and stability. It provides ample time to incorporate your feedback and carry out comprehensive quality assurance and testing processes before each release.

## Versioning System

Sysdig utilizes a semantic versioning system for the Sysdig Agent. Each version number is in the format of `X.Y.Z`, where `X` is the major version, `Y` is the minor version, and `Z` is the patch version. Here is what each version category signifies:

### Major Versions (X)

A change in the major version number signifies a significant update. Upgrading to a new major version might require changes to how the Sysdig Agent is used or configured. The release also often introduces major new features or changes in functionality.

### Minor Versions (Y) 

Minor versions typically introduce new features and enhancements. These updates expand the Sysdig Agent capabilities and improve its performance or usability without disrupting existing configurations or usage.

### Patch Versions (Z)

Patch versions are primarily focused on defect fixes and vulnerability fixes. They do not introduce new features but instead aim to improve the stability and reliability of the Sysdig Agent. These updates tend to be hotfixes released in between minor and major version releases to resolve issues as quickly as possible. These can typically be applied without any changes to the current usage or configuration.

See the [Sysdig Agent Releases Notes](/en/docs/release-notes/sysdig-agent-release-notes/) for specific changes introduced with each release.


## Version Support

Sysdig recommends that you utilize the latest version of the Sysdig Agent to ensure optimal performance and to have access to the latest features. 

If you continue to use an older version of the agent, Sysdig provides support for the `n-3` versions before the current one based on the minor number. 

For instance, if the latest release is `v12.8.0`, we will provide support for `n-3` versions backward, up to and including `v12.5.0`. 

Specifically: 

- For versions latest to `n-3`, Sysdig will provide deep troubleshooting and support for user-facing issues with the agent. 

- For versions **older than n-3**, Sysdig will provide expertise on known issues and best effort on new issues. The defects that need in-depth troubleshooting will require an upgrade to a supported version.

If an issue arises that is fixed in a newer version, you are expected to upgrade to the version with the fix. 
If a defect requires a code change, you must upgrade to the latest version to obtain the fix.

Sysdig does not provide hotfixes for versions older than the latest release. 

## Vulnerabilities

Sysdig takes security seriously and strives to ensure that our products and services are secure and protected from vulnerabilities. As part of our security measures, Sysdig has implemented a vulnerability management policy to address and fix vulnerabilities that are detected with Sysdig Agent promptly.

**Remediation Timeframe**

Depending on the CVE severity level, each level has a designated time frame for resolving the vulnerability, which starts when a fix is available for the vulnerability in a released package:

- Critical: 30 days
- High: 30 days
- Medium: 60 days

  
## Upgrading

When upgrading, we recommend that you upgrade to the latest version of the Sysdig Agent that is available to ensure that you can benefit from all the features, performance improvements, and defect fixes that Sysdig has to offer. 

Upgrading to a version older than the latest version available has no benefit. With every agent release, Sysdig conducts intensive regression testing to ensure no loss of existing functionality or performance.

## End of Support

At Sysdig, we have a 1-year deprecation policy for backend connectivity. This means that all the agent releases will have a 1-year deprecation policy moving forward. 

New prebuilt kernel probe binaries will only be generated for Sysdig Agent versions latest to n-3. Existing prebuilt kernel probes will remain available for older Sysdig Agent releases. If you require kernel probes for newer kernel versions, whilst using a version of agent `n-3` or older, you are required to build the kernel probes on the fly by using the kernel headers or the Sysdig probe builder.
