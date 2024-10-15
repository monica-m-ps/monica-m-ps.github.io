---
linkTitle: "Legacy Versions"
title: "Compliance Legacy Versions"
weight: "12"
aliases:
  - /en/Legacy-versions.html
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/benchmarks-legacy/
  - /en/benchmarks
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-legacy/
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/benchmarks-legacy/aws-foundations-benchmarks/
  - /en/compliance-legacy
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/benchmarks-legacy/review-benchmark-results/
  - /en/compliance-unified.html
  - /en/comp-unified
  - /en/docs/sysdig-secure/posture/compliance/compliance-unified-/
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/
  - /en/docs/sysdig-secure/posture/compliance/legacy-versions/compliance-unified/#enable-unified-compliance-ui/
Description: "If you are running older versions of Sysdig Secure, you might encounter different interactions of the Compliance UI and features, as well as the Benchmarks module, which in current versions has moved behind the scenes. The documentation appropriate for your Compliance tools depends on the software version you are running. "
---

{{< callout type="info" >}}

This version is retired for SaaS users from March 15, 2023. Benchmark results can be viewed in the [new compliance module](/en/docs/sysdig-secure/compliance/). 

The deprecation notice does not apply to on-prem users. 

{{% /callout %}}

In Jan. 2022, Sysdig Secure has unified and simplified the Compliance interface.

![](/image/comp_unified.png)

**From a single page,** you can now:

* **Scope all types of reports**

  * Scope across both host and cloud* platforms (workload*, Kubernetes, AWS, GCP, etc.)

  * Select any or all compliance frameworks (CIS AWS, CIS Azure, NIST, HIPAA, etc.)

  * Fine-tune selections by compliance framework version 

    ![](/image/comp_scope_blended.png)

* **Create/Enable/Disable reports**

  * Schedule a new report task for any of the available frameworks or platforms
  * Enable/disable existing tasks 

* **Review all scheduled tasks and the resulting reports**

  * At-a-glance summary of compliance status across the entire environment 

  * Click-down from the summary to review pass/fail/remediation details 

    ![](/image/comp_report_detail.png)

* **Benchmark tasks are now treated as just another compliance task, within the same interface**

  * No need to configure or reference the Legacy Benchmarks module once unified compliance is switched on

***Terminology note:** Compliance standards are scoped to different platforms depending on the specific security rules they include. Broadly, these are divided into:

-   **Workload** types: Including any Falco rules for kernel system calls, Falco rules for Kubernetes audit logs, host benchmarks, and security features that affect hosts, containers, and kubernetes clusters

-   **Cloud** type: Falco rules for CloudTrail and Cloud Custodian rules on AWS, or for GCP, Azure, and other cloud providers as they are added. 

## Enable Unified Compliance UI 

### Prerequisites 

* **Agent version >=** `12.0.4`

  If necessary, [install](/en/install-components-secure) or [upgrade](/en/upgrade-agents)  your agent to the appropriate version. 

* **Node analyzer** [installed](/en/install-secure)

When these two prerequisites are met, the UI for unified compliance will be automatically deployed. 

**NOTE:** If you are upgrading from an earlier version of Sysdig Secure, your existing compliance and benchmark records will be migrated to the new version and retained on the same schedule as before. 

## Use Compliance Reports 

### Access the Compliance Module 

Click the `Posture` icon in the left-hand navigation and select either `All Platforms` or an individual platform under `Compliance`. 

![](/image/comp_nav.png)

### Schedule New  Task

1. Click `+Schedule New` from the top-right corner of a Compliance landing page, or choose `Posture > New Report` from the nav bar. 

2. Choose the desired framework from the list presented and click `Schedule`.

   ![](/image/comp_frame.png)

   (Note that if a framework already has a scheduled task, you can view that report from here as well.) 

3. Configure the report details:

   ![](/image/comp_new.png)

   * **Report Name:** Assign a name to the scheduled task 
   * **Framework:** Auto-filled from the selection you made, or choose a different framework
   * **Version:** Select from the drop-down as needed 
   * **Platform:** Only applicable options will appear in the drop-down menu, based on the framework chosen
   * **Scope:** Select Entire Infrastructure or an appropriate subscope from the drop-down menu
   * **Schedule:** Choose Daily, Weekly, or Monthly and the time at which the task should be run and the report generated. 

4. Click **Schedule Report**.  At the designated schedule, the task will run and the report will be displayed on the Compliance landing page. 

### Review a Report

1. Navigate to the `Compliance` list from the `Posture` menu.

2. Select a report from the list to view the Report details. The top section of the page presents the compliance report summary, with the **Pass\|Fail summary data.**

   ![](/image/comp_report_detail.png)

   **Report Date:** The most current report is displayed; select a different date/time from the drop-down to see an earlier version. 

3. **Expand relevant details:** For example, click any *Failing Controls* in the summary at the top of the page and then expand to review the resources that are failing and find the suggested fixes. 

   ![](/image/comp1.png)

## Frameworks and Controls Implemented

{{< callout type="info" >}}

At this time, Charmed Kubernetes is not supported. 

{{% /callout %}}

### AWS Foundational Security Best Practices v1 (FSBP) Compliance

The [AWS Foundational Security Best Practices](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-standards-fsbp.html) standard is a set of controls that detect when your deployed accounts and resources deviate from security best practices. The standard allows you to continuously evaluate all of your AWS accounts and workloads to quickly identify areas of deviation from best practices. It provides actionable and prescriptive guidance on how to improve and maintain your organization's security posture. The controls include best practices from across multiple AWS services.

**For AWS protection,** Sysdig Secure will check the following sections:
AutoScaling.1, CloudTrail.1, Config.1, EC2.6, CloudTrail.2, DMS.1,
EC2.1, EC2.2, EC2.3, ES.1, IAM.1, IAM.2, IAM.4, IAM.5, IAM.6, IAM.7,
Lambda.2, GuardDuty.1

### AWS Well Architected Framework Compliance

The [AWS Well Architected Framework](https://aws.amazon.com/architecture/well-architected) helps cloud architects build secure, high-performing, resilient, and efficient infrastructure for a variety of applications and workloads. Built around six pillars—operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability—AWS Well-Architected provides a consistent approach for customers and partners to evaluate architectures and implement scalable designs.

**For workload protection,** Sysdig Secure will check the following
sections: OPS 4, OPS 5, OPS 6, OPS 7, OPS 8, SEC 1, SEC 5, SEC 6, SEC 7,
REL 2, REL 4, REL 5, REL 6, REL 10, PERF 5, PERF 6, PERF 7

**For AWS protection,** Sysdig Secure will check the following
sectionsOPS 6, SEC 1, SEC 2, SEC 3, SEC 8, SEC 9, REL 2, REL 9, REL 10

### FedRAMP Compliance

[FedRAMP](https://www.fedramp.gov/) is a government-wide program that promotes the adoption of secure cloud services across the federal government by providing a standardized approach to security and risk assessment for cloud technologies and federal agencies. FedRAMP empowers agencies to use modern cloud technologies, with an emphasis on security and protection of federal information.

**For workload protection,** Sysdig Secure will check the following sections:
AC-2, AC-2(4), AC-2(12), AC-6(1), AC-6(2), AC-6(3), AU-2, AU-6,
AU-10, AU-12, CM-3(6), CM-7, CM-7(1), SA-10, SC-8, SC-8(1), SI-3, SI-4(4)

**For AWS protection,** Sysdig Secure will check the following sections:
AC-2, AC-2(4), AU-8, SC-8(1)

**For Google Cloud protection,** Sysdig Secure will check the following sections:
AC-6(1), AC-6(2), AC-6(3), AU-9(2), AU-12(1), CM-3(1), SC-7(4)

**For Azure protection,** Sysdig Secure will check the following sections:
AC-2, AU-8

### GDPR Compliance

The [General Data Protection Regulation 2016/679 (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection/data-protection-eu_en) is
a regulation for data protection and privacy in the European Union (EU)
and the European Economic Area (EEA). It also addresses the transfer of
personal data outside the EU and EEA areas.

**For workload protection,** Sysdig Secure will check the following
sections: 5.1, 5.2, 24.1, 24.2, 24.3, 25.1, 25.2, 25.3, 32.1, 32.2, 40.2

**For AWS protection,** Sysdig Secure will check the following sections:
5.1, 5.2, 24.1, 24.2, 24.3, 25.1, 25.2, 25.3, 30.1, 30.2, 30.3, 30.4,
30.5, 32.1, 32.2, 40.2

**For Google Cloud protection,** Sysdig Secure will check the following sections:
25.1, 25.2, 25.3, 32.1, 32.2

**For Azure protection,** Sysdig Secure will check the following sections:
25.1, 25.2, 25.3, 32.1, 32.2

### HIPAA Compliance

The [Health Insurance Portability and Accountability Act (HIPAA)](https://www.hhs.gov/hipaa/index.html) sets the standard for protecting sensitive patient data. Companies dealing with Protected Health Information (PHI) must have and comply with physical, network, and technology security measures to maintain HIPAA compliance. Any entity providing health care treatment, payment, and operations, as weel as any entity who has access to patient information and provides support for treatment, payment, or operations must comply with HIPAA requirements. Other organizations such as subcontractors and any other related business partners must also comply.

**For workload protection,** Sysdig Secure will check the following
sections: 164.308(a)(1)(ii)(D), 164.308(a)(3)(i), 164.308(a)(3)(ii)(A),
164.308(a)(3)(ii)(B), 164.308(a)(4)(i), 164.308(a)(4)(ii)(A),
164.308(a)(4)(ii)(B), 164.308(a)(4)(ii)(C), 164.308(a)(5)(ii)(B),
164.308(a)(5)(ii)(C), 164.310(a)(2)(iii), 164.310(b), 164.312(a)(1),
164.312(a)(2)(i), 164.312(a)(2)(ii), 164.312(a)(2)(iv), 164.312(b),
164.312(c)(1), 164.312(c)(2), 164.312(d), 164.312(e)(2)(i),
164.312(e)(2)(ii)

**For AWS protection,** Sysdig Secure will check the following sections:
164.308(a)(1)(ii)(D), 164.308(a)(3)(i), 164.308(a)(3)(ii)(A),
164.308(a)(3)(ii)(B), 164.308(a)(4)(i), 164.308(a)(4)(ii)(A),
164.308(a)(4)(ii)(B), 164.308(a)(4)(ii)(C), 164.308(a)(5)(ii)(B),
164.308(a)(5)(ii)(C), 164.308(a)(8), 164.310(b), 164.312(a)(1),
164.312(a)(2)(i), 164.312(a)(2)(ii), 164.312(b), 164.312(c)(1),
164.312(c)(2), 164.312(e)(2)(i)

**For Google Cloud protection,** Sysdig Secure will check the following sections:
164.310(b), 164.312(b), 164.312(d)

### HITRUST CSF v9.4.2 Compliance

The [HITRUST Common Security Framework (CSF)](https://hitrustalliance.net/product-tool/hitrust-csf/) provides the structure, transparency, guidance, and cross-references to authoritative sources organizations globally need to be certain of their data protection compliance. It leverages nationally and internationally accepted security and privacy-related regulations, standards, and frameworks–including ISO, NIST, PCI, HIPAA, and GDPR–to ensure a comprehensive set of security and privacy controls and continually incorporates additional authoritative sources. The HITRUST CSF standardizes these requirements, providing clarity and consistency and reducing the burden of compliance.

**For workload protection,** Sysdig Secure will check the following sections:
01.b, 01.c, 01.i, 01,j, 01.k, 01.l, 01.m, 01.n, 01.o, 01.p, 01.q, 01.s, 01.v, 01.w,
01.x, 01.y, 03.d, 05.i, 06.h, 06.i, 06.j, 09.b, 09.i, 09.j, 09.k, 09.m, 09.n, 09.s,
09.v, 09.w, 09.x, 09.y, 09.z, 09.aa, 09.ab, 09.ac, 09.ad, 09.ae, 10.c, 10.d, 10.g,
10.h, 10.j, 10.k, 10.m, 11.a, 11.b

**For AWS protection,** Sysdig Secure will check the following sections:
01.c, 01.i, 01.p, 01.s, 01.v, 01.x, 01.y, 05.i, 06.i, 09.m, 
09.v, 09.x, 09.ac, 09.af, 10.j, 11.b

**For Google Cloud protection,** Sysdig Secure will check the following sections:
01.c, 01,j, 01.n, 01.q, 01.y, 05.i, 06.d, 06.j, 09.m, 09.s,
10.g, 10.k

**For Azure protection,** Sysdig Secure will check the following sections:
01.x, 09.m, 09.ac, 09.af, 11.b

### ISO 27001:2013 Compliance

The [ISO/IEC 27001:2013](https://www.iso.org/standard/54534.html) is an international standard on how to manage information security. It details requirements for establishing, implementing, maintaining and continually improving an information security management system (ISMS).

**For workload protection,** Sysdig Secure will check the following
sections: A.6.1.2, A.8.1.1, A.8.1.2, A.8.1.3, A.9.1.2, A.9.2.3, A.9.4.1,
A.9.4.4, A.10.1.1, A.12.1.2, A.12.4.1, A.12.5.1, A.12.6.1, A.12.6.2,
A.13.1.1, A.13.1.2, A.13.1.3, A.14.1.2, A.14.2.2, A.14.2.4, A.18.1.3,
A.18.1.5

**For AWS protection,** Sysdig Secure will check the following sections:
A.6.1.2, A.9.1.1, A.9.1.2, A.9.2.3, A.9.2.5, A.9.4.2, A.9.4.3, A.10.1.1,
A.10.1.2, A.12.1.2, A.13.1.1, A.14.1.2, A.18.1.3, A.18.1.5

**For Google Cloud protection,** Sysdig Secure will check the following sections:
A.6.1.2, A.9.1.2, A.9.2.3, A.10.1.2, A.18.1.3, A.18.1.5

**For Azure protection,** Sysdig Secure will check the following sections:
A.9.1.2, A.9.4.2, A.10.1.1, A.13.1.1, A.14.1.2, A.18.1.3, A.18.1.5

### NIST 800-53 rev4 and rev5 Compliance

The National Institute of Standards and Technology (NIST) [Special Publication 800-53 revision 4](https://nvd.nist.gov/800-53)
describes the full range of controls required to pass a NIST 800-53 audit.

**For workload protection,** Sysdig Secure will check the following
sections: AC-2, AC-2(4), AC-2(12), AC-3, AC-4, AC-4(17), AC-6, AC-6(1),
AC-6(2), AC-6(3), AC-6(5), AC-6(6), AC-6(9), AC-6(10), AC-14, AC-17,
AC-17(1), AC-17(3), AC-17(4), AU-2, AU-6, AU-6(8), AU-10, AU-12, CA-9,
CM-3, CM-3(6), CM-5, CM-7, CM-7(1), CM-7(4), IA-3, SA-10, SA-15(10),
SC-2, SC-4, SC-7, SC-7(3), SC-7(10), SC-8, SC-8(1), SC-12(3), SC-17,
SC-39, SI-3, SI-3(1), SI-3(2), SI-4, SI-4(2), SI-4(4), SI-4(11),
SI-4(13), SI-4(18), SI-4(20), SI-4(22), SI-4(23), SI-4(24), SI-7,
SI-7(3), SI-7(9), SI-7(11), SI-7(12), SI-7(13), SI-7(14), SI-7(15)

**For AWS protection,** Sysdig Secure will check the following sections:
AC-2, AC-2(4), AC-4, AC-6, AC-6(9), AU-6(8), AU-8, CA-7, CM-6, SC-8(1),
SI-4, SI-12

**For Google Cloud protection,** Sysdig Secure will check the following sections:
AC-6(1), AC-6(2), AC-6(3), AC-6(5), AC-6(9), AC-6(10), AC-17(1), AC-17(2),
AC-17(3), AU-6(8), AU-9(2), AU-12(1), CM-3(1), IA-2(12), SC-7(3),
SC-7(4), SC-7(5), SC-7(8), SC-7(21), SC-12(1)

**For Azure protection,** Sysdig Secure will check the following sections:
AC-2, AU-8, SI-4

[Special Publication 800-53 revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
was published in September 2020 and includes some modifications. For 12 months
both revisions will be valid, and revision 4 will be deprecated in September 2021.

**For workload protection,** Sysdig Secure will check the following
sections: AC-2, AC-2(4), AC-2(12), AC-3, AC-4, AC-4(17), AC-6, AC-6(1),
AC-6(2), AC-6(3), AC-6(5), AC-6(6), AC-6(9), AC-6(10), AC-14, AC-17,
AC-17(1), AC-17(3), AC-17(4), AC-17(10), AU-2, AU-6, AU-6(8), AU-10,
AU-12, CA-3(6), CA-7(4), CA-7(5), CA-9, CM-3, CM-3(6), CM-3(7), CM-3(8),
CM-4, CM-4(2), CM-5, CM-5(1), CM-7, CM-7(1), CM-7(4), CM-7(6), CM-7(7),
CM-7(8), CM-8, CM-11(3), IA-3, MA-3(5), MA-3(6), PM-5(1), RA-3(4),
RA-10, SA-10, SA-15(10), SA-23, SC-2, SC-4, SC-7, SC-7(3), SC-7(10),
SC-7(25), SC-7(26), SC-7(27), SC-7(28), SC-7(29), SC-8, SC-8(1),
SC-12(3), SC-17, SC-39, SC-50, SI-3, SI-4, SI-4(2), SI-4(4), SI-4(11),
SI-4(13), SI-4(18), SI-4(20), SI-4(22), SI-4(23), SI-4(24), SI-4(25),
SI-7, SI-7(3), SI-7(9), SI-7(12), SI-7(15)

**For AWS protection,** Sysdig Secure will check the following sections:
AC-2, AC-2(4), AC-4, AC-6, AC-6(9), AU-6(8), AU-8, SC-8(1), SI-4

**For Google Cloud protection,** Sysdig Secure will check the following sections:
AC-6(1), AC-6(2), AC-6(3), AC-6(5), AC-6(9), AC-6(10), AC-17(1), AC-17(2),
AC-17(3), AU-6(8), AU-9(2), AU-12(1), CM-3(1), IA-2(12), SC-7(3),
SC-7(4), SC-7(5), SC-7(8), SC-7(21), SC-12(1)

**For Azure protection,** Sysdig Secure will check the following sections:
AC-2, AU-8, SI-4

### NIST 800-82 rev2 Compliance

The National Institute of Standards and Technology (NIST)
[Special Publication 800-82 revision 2](https://csrc.nist.gov/publications/detail/sp/800-82/rev-2/final)
provides guidance on how to secure Industrial Control Systems (ICS),
including Supervisory Control and Data Acquisition (SCADA) systems, Distributed
Control Systems (DCS), and other control system configurations such as Programmable
Logic Controllers (PLC), while addressing their unique performance, reliability,
and safety requirements.

**For workload protection,** Sysdig Secure will check the following
sections: AC-2, AC-2(4), AC-2(12), AC-3, AC-4, AC-6, AC-6(1),
AC-6(2), AC-6(3), AC-6(5), AC-6(9), AC-6(10), AC-17,
AC-17(1), AC-17(3), AC-17(4), AU-2, AU-6, AU-10,
AU-12, CA-9, CM-3, CM-5, CM-7, CM-7(1), IA-3, SA-10, SC-2, SC-4, SC-7, SC-7(3),
SC-8, SC-8(1), SC-17, SC-39, SI-3, SI-3(1), SI-3(2), SI-4, SI-4(2), SI-4(4), 
SI-7, SI-7(14)

**For AWS protection,** Sysdig Secure will check the following sections:
AC-2, AC-2(4), AC-4, AC-6, AC-6(9), AU-8, SC-8(1), SI-4.

**For Google Cloud protection,** Sysdig Secure will check the following
sections: AC-6(1), AC-6(2), AC-6(3), AC-6(5), AC-6(9), AC-6(10), 
AC-17(1), AC-17(2), AC-17(3), AU-9(2), AU-12(1),
CM-3(1), IA-2(12), SC-7(3), SC-7(4), SC-7(5), SC-7(8), SC-7(21), 
SC-12(1)

**For Azure protection,** Sysdig Secure will check the following sections:
AC-2, AU-8, SI-4

### NIST 800-171 rev2 Compliance

The National Institute of Standards and Technology (NIST)
[Special Publication 800-171 revision 2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final) 
provides agencies with recommended security requirements for
protecting the confidentiality of Controlled Unclassified Information
(CUI) when the information is resident in nonfederal systems and
organizations.

**For workload protection,** Sysdig Secure will check the following
sections:3.1.1, 3.1.2, 3.1.3, 3.1.5, 3.1.6, 3.1.7, 3.1.12, 3.1.13,
3.1.14, 3.1.15, 3.1.16, 3.1.17, 3.1.20, 3.3.1, 3.3.2, 3.3.5, 3.3.8,
3.3.9, 3.4.3, 3.4.5, 3.4.6, 3.4.7, 3.4.9, 3.5.1, 3.5.2, 3.11.2, 3.12.1,
3.13.1, 3.13.2, 3.13.3, 3.13.4, 3.13.5, 3.13.6, 3.13.8, 3.14.1, 3.14.2,
3.14.3, 3.14.4, 3.14.5, 3.14.6, 3.14.7

**For AWS protection,** Sysdig Secure will check the following
sections:3.1.1, 3.1.2, 3.1.3, 3.3.1, 3.3.2, 3.3.7, 3.5.7, 3.5.8, 3.14.6,
3.14.7

**For Azure protection,** Sysdig Secure will check the following sections:
3.1.1, 3.1.2, 3.3.7, 3.14.6, 3.14.7

### NIST 800-190 Compliance

The National Institute of Standards and Technology (NIST) [Special
Publication 800-190](https://csrc.nist.gov/publications/detail/sp/800-190/final) 
explains the potential security concerns associated with the use of
containers and provides recommendations for addressing these concerns.

**For workload protection,** Sysdig Secure will check the following sections:
3.1.1, 3.1.2, 3.1.3, 3.1.4, 3.1.5, 3.2.1, 3.2.2, 3.3.1, 3.3.2,
3.3.3, 3.3.4, 3.3.5, 3.4.1, 3.4.2, 3.4.3, 3.4.4, 3.4.5, 3.5.2, 3.5.5

### PCI DSS v3.2.1

The [PCI Data Secirity Standard (DSS) Quick Reference](https://www.pcisecuritystandards.org/documents/PCI_DSS-QRG-v3_2_1.pdf)
describes the full range of controls required to pass a PCI 3.2 audit. In this
release, Sysdig Secure will check the following subset:

**For workload protection:** 1.1.2, 1.1.3, 1.1.5, 1.1.6.b,
2.2, 2.2.a, 2.2.1, 2.2.2, 2.4, 2.6, 4.1, 6.1, 6.2, 6.4.2, 6.5.1, 6.5.6,
6.5.8, 7.2.3, 10.1, 10.2, 10.2.1, 10.2.5, 10.2.7, 10.5.5, 11.5.1

**For AWS protection,** Sysdig Secure will check the following
sections: 2.2, 2.2.2, 10.1, 10.2.1, 10.2.2, 10.2.5, 10.2.6, 10.2.7,
10.5.5, 11.4

**For Google Cloud protection,** Sysdig Secure will check the following
sections: 1.1.5, 7.1.2, 10.1, 10.2, 10.3

**For Azure protection,** Sysdig Secure will check the following
sections: 2.2.2

### SOC2  

The [American Institute of CPAs (AICPA)](https://future.aicpa.org/home) describes the full range of controls required to pass a SOC 2 audit.

**For workload protection,** Sysdig Secure will check the following
sections: CC3.2, CC5.1, CC5.2, CC6.1, CC6.2, CC6.6, CC6.8, CC7.1, CC7.2,
CC7.5, CC8.1, CC9.1

**For AWS protection,** Sysdig Secure will check the following sections:
CC3.2, CC5.2, CC6.2, CC6.6, CC7.1, CC7.2

**For Google Cloud protection,** Sysdig Secure will check the following sections:
CC5.2, CC6.1, CC6.2, CC6.6, CC7.1, CC8.1

**For Azure protection,** Sysdig Secure will check the following sections:
CC5.2, CC6.1, CC6.6, CC7.2, CC8.1

## History of Compliance Components

* [Compliance/Actionable Compliance](/en/docs/sysdig-secure/posture/compliance/): Offered for early review on May 31, 2022, now GA and called "Compliance"


* Compliance (Legacy): Deprecated
* Benchmarks (Legacy) and Benchmarks (v1): Deprecated

## Migration Guide 

For users migrating to the new Compliance module, released January, 2023: 

* Starting January 17th, SaaS customers that connect new data sources for Sysdig cloud accounts or Sysdig agents will automatically have the new Compliance module (previously known as “Actionable Compliance”) enabled.  Resources of the connected data sources  will be evaluated according to CSPM/Risk and Compliance policies that are applied on zones.  Results are displayed about 5-10 minutes after connection, varying by the scale of the resources. 

* If you were using Unified Compliance:
  * For Existing Kubernetes clusters: ensure that your applied helm charts are updated according to the [ KSPM Components](/en/docs/installation/kspm-components/) guide.
  * For Existing GCP cloud accounts, enable the [Cloud Asset API](https://console.cloud.google.com/marketplace/product/google/cloudasset.googleapis.com).
  * The new Compliance module will be auto-enabled on your existing Cloud accounts by January 26th. 

* Currently, the CSPM Compliance module is not available for On-premises and IBM Cloud users who can continue using Unified Compliance.













