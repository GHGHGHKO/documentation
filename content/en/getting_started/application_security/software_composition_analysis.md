---
title: Getting Started with Software Composition Analysis
aliases:
- /getting_started/application_security/vulnerability_management
further_reading:
- link: "https://www.datadoghq.com/blog/datadog-software-composition-analysis/"
  tag: "Blog"
  text: "Mitigate vulnerabilities from third-party libraries with Datadog Software Composition Analysis"
- link: "/security/code_security/software_composition_analysis/"
  tag: "Documentation"
  text: "Read more about Software Composition Analysis in source code"
- link: "/security/code_security/software_composition_analysis"
  tag: "Documentation"
  text: "Read more about Software Composition Analysis in ASM libraries"
- link: "/security/application_security/how-appsec-works"
  tag: "Documentation"
  text: "How Application Security Management works"
- link: "https://securitylabs.datadoghq.com/"
  tag: "Security Labs"
  text: "Security research, reports, tips, and videos from Datadog"
- link: "https://www.datadoghq.com/blog/sca-supply-chain-security/"
  tag: "Blog"
  text: "Beyond vulnerabilities, towards a holistic approach to securing the software supply chain"
---


## Overview

Datadog [Software Composition Analysis][15] (SCA) continuously monitors your production environment for vulnerabilities in the open source libraries your applications rely on. You can identify and prioritize the remediation of the highest vulnerabilities by business impact.

This guide walks you through best practices for getting your team up and running with SCA.

## Phase 1: Enable

First, see the [Library Compatibility][12] requirements page to verify if the Datadog Tracing Library used by your application or service supports the Software Composition Analysis (SCA) capability for your application's or service's programming language.

### Enable SCA on your services using the Quick Start Guide

1. In Datadog, go to **[Application Security > Settings > Quick Start Guide][4]**.
2. Expand **Enable Vulnerability Detection**, select **Open source vulnerabilities**, and click **Start Activation**. A list of services appears.
3. Select the service(s) you want to monitor for vulnerabilities, then click **Next**. The number of selected services and their names are listed.
4. Click **Enable for Selected Service(s)** to complete the activation of Software Composition Analysis (SCA) for the chosen service(s).
   
   
   {{< img src="/code_security/software_composition_analysis/APM_SCA-enablement-quick-start-guide.mp4" alt="quick start guide in the Datadog UI" video="true">}}

### Enable SCA on your repositories and services using the Settings page

1. In Datadog, go to **[Application Security > Settings][13]**.
2. Click **Get Started** to expand the Software Composition Analysis (SCA) capability.


#### Enable SCA on GitHub repositories

1. Click **Select Repositories** on your desired GitHub account and toggle **Enable Software Composition Analysis (SCA)** to enable for all repositories. If you do not see any GitHub accounts listed, [create a new GitHub App][14] to get started.
   
{{< img src="/code_security/software_composition_analysis/SCA-github-all-repositories.png" alt="enable SCA for all repositories">}}
   
Optionally, you can select specific GitHub repositories to enable SCA by clicking the toggle for each repository.
   
{{< img src="/code_security/software_composition_analysis/SCA-github-enabled-repositories.png" alt="enable SCA for all repositories" style="width:100%;" >}}

#### Enable SCA on services

1. Click **Select Services**. A list of services should appear.
2. Select the service(s) you want to monitor for vulnerabilities, then click **Next**. You should see the number of selected services and their names.
3. Click **Enable for Selected Service(s)** to complete the activation of Software Composition Analysis (SCA) for the chosen service(s).
<br>      


{{< img src="/code_security/software_composition_analysis/SCA-enablement-settings-services.mp4" alt="SCA enablement in the Datadog UI" video="true">}}

## Phase 2: Identify
1. **Identify Vulnerabilities**: Navigate to [Vulnerabilities][5].  
   - Sort by `Status`, `Vulnerability Source`, and `Severity`.
   - To switch to the code repository commit point of view, click on the **static** button. To switch to the real-time point of view to the applications already running, click on the **runtime** button.

   {{< img src="/code_security/software_composition_analysis/asm_sca_vulnerabilities_2.png" alt="Software Composition Analysis (SCA) explorer page showing vulnerabilities sorted by static or runtime." style="width:100%;" >}}

   Each vulnerability has its own status to help prioritize and manage findings:

   | Status         | Description                                                                                   |
   | -------------- | ----------------------------------------------------------------------------------------------| 
   |  Open          |  The vulnerability has been detected by Datadog.                                              |
   |  In Progress   |  A user has marked the vulnerability as In Progress, but Datadog still detects it.            |
   |  Muted         |  A user has ignored the vulnerability, making it no longer visible on the Open list, but Datadog still detects it. |
   |  Remediated    |  A user has marked the vulnerability as resolved, but Datadog still sees the vulnerability.   |
   |  Auto-Closed   |  The vulnerability is no longer detected by Datadog.                                          |                              

   **Note**: Remediated and Auto-Closed vulnerabilities re-open if the vulnerability is detected again by Datadog.

2. View additional details by clicking on the vulnerability. This opens a panel which includes information about:
    - Which services are affected.
    - The date on which the vulnerability was last detected.
    - A description of the vulnerability.
    - Recommended remediation steps.
    - Vulnerability score. </br> </br>

      {{< img src="getting_started/appsec/appsec-vuln-explorer_3.png" alt="Application Vulnerability Management detailed view of the vulnerability." style="width:100%;" >}}

      **Note**: The severity of a vulnerability within SCA is modified from the base score to take into account the presence of attacks and the business sensitivity of the environment where the vulnerability is detected. For example, if no production environment is detected, the severity is reduced.</br> </br>

      The adjusted vulnerability score includes the full context of each service:
        - The original vulnerability severity.
        - Evidence of suspicious requests.
        - Sensitive or internet-exposed environments.

      Severities are scored by the following:
      | CVSS Score    | Qualitative Rating
      | --------------| -------------------|  
      |   `0.0`         | None                |
      |   `0.1 - 3.9`   | Low                 |
      |   `4.0 - 6.9`   | Medium              |
      |   `7.0 – 8.9`   | High                |
      |   `9.0 – 10.0`  | Critical            |

3. Optionally, download the library inventory (list of libraries and versions in CycloneDX format) for your service. While viewing the details of a vulnerability, click [View in Software Catalog][6]. From here you can navigate to the [Security view][7] of your service, and download the library inventory under the [libraries tab][8]. 

## Phase 3: Remediate
1. **Prioritize Response and Remediate**: While on the [Vulnerability Explorer][5], take action:

    - Change the status of a vulnerability.
    - Assign it to a team member for further review.
    - Create a Jira issue. To create Jira issues for SCA vulnerabilities, you must configure the Jira integration, and have the `manage_integrations` permission. For detailed instructions, see the [Jira integration][11] documentation, as well as the [Role Based Access Control][10] documentation.
    - Review recommended remediation steps.
    - View links and information sources to understand the context behind each vulnerability.

   **Note**: Adding an assignee to the vulnerability does not generate a notification regarding the assignment. This action only lists their name as an annotation of the vulnerability.

   {{< img src="getting_started/appsec/appsec-vuln-remediation_3.png" alt="Application Vulnerability Management recommended remediation steps of the vulnerability." style="width:100%;" >}}

## Disable SCA

For information on disabling Software Composition Analysis, see [Disabling Software Composition Analysis][16].

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /security/code_security/software_composition_analysis/
[4]: https://app.datadoghq.com/security/configuration/asm/onboarding
[5]: https://app.datadoghq.com/security/appsec/vm
[6]: https://app.datadoghq.com/services
[7]: /tracing/software_catalog/#security-view
[8]: /tracing/software_catalog/#investigate-a-service
[9]: https://app.datadoghq.com/security/configuration/asm/setup
[10]: /account_management/rbac/permissions/#integrations
[11]: /integrations/jira/
[12]: https://app.datadoghq.com/security/configuration/asm/onboarding
[13]: https://app.datadoghq.com/security/configuration/asm/setup
[14]: https://docs.datadoghq.com/integrations/github/
[15]: /security/code_security/software_composition_analysis/setup_runtime/compatibility/
[16]: /security/application_security/troubleshooting/#disabling-software-composition-analysis