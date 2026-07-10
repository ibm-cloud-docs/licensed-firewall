---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate planning, licensed firewall planning, FortiGate limitations, security group, Fortinet notifications

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Planning for FortiGate on IBM Cloud VPC
{: #planning}

Before you deploy a FortiGate firewall, review the following topics and considerations to ensure that your configuration meets your security, compliance, and operational requirements.
{: shortdesc}

## Before you deploy
{: #planning-before-you-deploy}

Ensure that the following resources exist in your IBM Cloud account before you deploy:

- An IBM Cloud account with access to VPC
- A VPC with subnets configured for your deployment
- Appropriate IAM permissions to create and manage resources
- A secure access method (such as VPN or bastion host) to reach the firewall instance

Review the following topics before you deploy. Understanding your options upfront helps you avoid configuration decisions after deployment that require redeployment.

- [Review license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles) — Understand the available license tiers, deployment sizes, and performance characteristics.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — Review the bootstrap configurations applied to each of the five deployment options (Single VM, HA single-zone active, HA single-zone passive, HA cross-zone active, HA cross-zone passive).

## Planning considerations
{: #planning-considerations}

Review the following important considerations before you order:

- **License plan is permanent** — The license plan that you select at deployment cannot be changed after provisioning. You are billed for that license on the virtual server instance for as long as it runs. If you need a different license plan, you must place a new order and cancel the existing deployment. You can resize the virtual server instance without changing the license. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
- **Security group denies all inbound traffic by default** — A floating IP is automatically assigned to `port1` (the public-facing interface) and is visible in your VPC resources immediately after deployment. However, you cannot connect to the firewall until you add an inbound security group rule that allows management access from your administrator IP address. For more information, see [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).
- **Subscribe to Fortinet security notifications** — Stay informed about vulnerabilities, patches, and firmware updates. For more information, see [Subscribing to Fortinet notifications](/docs/licensed-firewall?topic=licensed-firewall-security-maintenance-vulnerability-management). Keeping your FortiGate firmware up to date is your responsibility and is essential to maintaining a secure posture.

## Limitations
{: #planning-limitations}

The following limitations apply to this offering. Review them before you deploy.

- **Regional availability varies** — The ATP license plan uses gen2-cx instance profiles, which are not available in all regions. Specifically, the `cx2-2x4` and `cx2-8x16` profiles are not available in Mumbai, Chennai, and Montreal. Check the [IBM Cloud catalog](https://cloud.ibm.com/catalog){: external} for supported locations before ordering.
- **FortiGate Cloud and FortiCloud access is not available** — Because the license is managed through the IBM Fortinet account, access to FortiGate Cloud and FortiCloud is not supported. AI-based inline malware prevention (Enterprise license) can be configured and used locally but cannot connect to FortiGate Cloud services.
- **FortiManager and FortiAnalyzer are not included** — The standard offering does not include FortiManager or FortiAnalyzer. You can deploy these separately in your VPC if centralized management or advanced analytics are required.
- **This is not a managed service** — IBM manages licensing and provides support coordination, but you are responsible for deploying, configuring, and maintaining your FortiGate instances. IBM does not configure firewall policies or apply updates on your behalf. For more information, see [Shared responsibilities](/docs/licensed-firewall?topic=licensed-firewall-shared-responsibilities).

## Known issues
{: #planning-known-issues}

Known issues are identified bugs or unexpected behaviors that were not fixed before release, but weren't critical enough to delay it. These issues are communicated to you, often with workarounds, and are prioritized for resolution in the near term by the development team.

- ?
- ?
- ?
