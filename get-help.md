---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate support, licensed firewall help, open support case, firewall troubleshooting

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}



# Getting help and support for FortiGate licensed firewall
{: #help-and-support}

If you experience an issue or have questions when using the FortiGate licensed firewall, you can use the following resources before you open a support case.
{: shortdesc}

* Ask a question in the [AI assistant](/docs/overview?topic=overview-ask-ai-assistant) from the console or the {{site.data.keyword.cloud_notm}} CLI.
* Review the [FAQs](/docs/licensed-firewall?topic=licensed-firewall-my-service-faq) in the product documentation.
* Review the [troubleshooting documentation](/docs/licensed-firewall?topic=licensed-firewall-troubleshoot-licensed-firewall) to troubleshoot and resolve common issues.
* Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](/status){: external}.
* Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. When you ask a question, tag the question with `ibm-cloud` and `fortigate`, so that it's seen by the {{site.data.keyword.Bluemix_notm}} development teams.

If you still can't resolve the problem, you can open a support case. For more information, see [Creating support cases](/docs/support?topic=support-open-case&interface=ui). And if you're looking to provide feedback, see [Submitting feedback](/docs/overview?topic=overview-feedback).

## Providing support case details
{: #support-case-details}

To ensure that the support team can start investigating your case and provide a timely resolution, include the following information when you open a support case for the FortiGate licensed firewall.

1. Provide your firewall instance details:
   * The virtual server instance ID and name for the FortiGate instance.
   * The VPC ID and region where the firewall is deployed.
   * The Schematics workspace ID used to deploy the firewall (if applicable).
   * The license plan and deployment size (for example, Enterprise, Large).

2. Describe the issue:
   * Steps to reproduce the problem.
   * Any error messages displayed in the FortiGate web console or IBM Cloud console.
   * Relevant FortiGate log output (available under **Log & Report** in the FortiGate web console).

3. Provide network details if the issue involves connectivity:
   * Source and destination IP addresses.
   * The subnet IDs for `port1` and `port2`.
   * Any security group rules that might be relevant.
