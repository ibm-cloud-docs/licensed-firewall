---

copyright:
  years: 2026

lastupdated: "2026-09-29"

keywords: FortiGate, IBM Cloud VPC, licensed firewall, fortinet, next-generation firewall, NGFW, paygo firewall

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Fortinet FortiGate for IBM Cloud VPC
{: #getting-started}

IBM Cloud Virtual Private Cloud (VPC) provides a scalable and secure foundation for hosting modern cloud workloads. Within this environment, Fortinet FortiGate next-generation firewall technology delivers complete content and network protection and is available for deployment on IBM Cloud VPC.
{: shortdesc}

Deployed in a VPC architecture, FortiGate VM provides centralized visibility and control over network traffic entering, leaving, and moving within the virtual private network. This enables organizations to apply consistent security policies, improve workload segmentation, and protect applications from a wide range of evolving cyberthreats while maintaining cloud agility and scalability.

Because FortiGate VM is deployed as a licensed virtual appliance in IBM Cloud VPC, organizations must also account for resource consumption, such as compute, storage, and network usage, to ensure effective monitoring and cost control.

**Disclaimer:** This third-party product is provided by a vendor outside of IBM and is subject to a separate agreement between you and the third party, if you accept their terms. IBM is not responsible for the product and makes no privacy, security, performance, support, or other commitments regarding the product, unless otherwise noted in the provided terms.
{: important}

## Key benefits
{: #fortigate-highlights}

FortiGate VM PayGo combines enterprise-grade security with cloud-native simplicity, helping you remove common deployment and operational barriers.

- **Deploy instantly** – Launch firewalls directly from the IBM Cloud catalog with no procurement or setup delays, that use the same workflows as the rest of your cloud infrastructure.
- **Pay as you go** – Align costs to actual usage with a flexible pay-as-you-go model, with no upfront commitment.
- **No license management** – Licensing is automatically applied at provisioning time. Eliminate renewals, tracking, and administrative costs.
- **Scale on demand** – Adjust capacity and security features as workloads change.
- **Centralized visibility and control** – Monitor and control all network traffic entering, leaving, and moving within your VPC from a single management interface.
- **Comprehensive built-in security** – Protect workloads with IPS, application-aware policy enforcement, antivirus, web filtering, VPN, and continuous threat intelligence updates.

## Getting started
{: #getting-started-next-steps}

To get started, follow these steps:

1. Review [Planning for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-planning): Verify account prerequisites, review license plans and instance profiles, understand the default firewall configuration, and review deployment constraints and limitations before you order.
1. [Deploy a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-single-vm): Deploy a single VM, HA single zone, or HA cross-zone configuration from the IBM Cloud catalog.
1. [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall): Add an inbound security group rule to allow management access, configure VPC routing to pass traffic through the firewall, and log in to the management interface for the first time. Step-by-step instructions for each of these tasks are provided in that topic.
1. [Configure your FortiGate firewall after deployment](/docs/licensed-firewall?topic=licensed-firewall-configuring-fortigate): Change the administrator password, verify interfaces, create firewall policies, and configure routing.
1. [Enable security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services): Activate Intrusion Prevention System (IPS), antivirus, web filtering, and other security profiles based on your license plan.

   For security hardening guidance after initial setup, see [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices).
   {: note}

1. [Back up the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config): Create a backup of your configuration after you complete your initial configuration and store it securely.
1. [Monitor your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-monitoring): Review logs, check instance health, and verify that firewall policies are working as expected.
1. [Manage firmware updates and vulnerability patches](/docs/licensed-firewall?topic=licensed-firewall-addressing-vulnerabilities): Subscribe to Fortinet security advisories, assess the impact on your deployment, back up your configuration, and apply firmware updates to keep your firewall secure and up to date.

If you are migrating an existing classic infrastructure FortiGate deployment to VPC, see [Migrating to a virtual firewall in VPC](/docs/classic-to-vpc?topic=classic-to-vpc-vpc-firewall-options) for available deployment patterns, including stand-alone, Active/Passive, and Active/Active configurations, and integration with VPC networking constructs, such as the SDN Connector and public address ranges.
{: attention}

## Related reference
{: #getting-started-related-references}

- [Understanding FortiGate licensing](/docs/licensed-firewall?topic=licensed-firewall-understanding-fortigate-licensing): How licensing is activated on initial startup, public connectivity requirements per topology, and license status verification.
