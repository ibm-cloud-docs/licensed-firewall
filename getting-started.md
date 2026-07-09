---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate, IBM Cloud VPC, licensed firewall, fortinet, next-generation firewall, NGFW, paygo firewall

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Fortinet FortiGate for IBM Cloud VPC
{: #getting-started}

IBM Cloud Virtual Private Cloud (VPC) provides a scalable and secure foundation for hosting modern cloud workloads. Within this environment, Fortinet FortiGate next-generation firewall technology delivers complete content and network protection and is available for deployment on IBM Cloud VPC.
{: shortdesc}

Deployed in a VPC architecture, FortiGate provides centralized visibility and control over network traffic entering, leaving, and moving within the virtual private network. This enables organizations to apply consistent security policies, improve workload segmentation, and protect applications from a wide range of evolving cyber threats while maintaining cloud agility and scalability.

Because FortiGate is deployed as a licensed virtual appliance in IBM Cloud VPC, organizations must also account for resource consumption, such as compute, storage, and network usage, to ensure effective monitoring and cost control.

## Key benefits
{: #fortigate-highlights}

FortiGate PayGo combines enterprise-grade security with cloud-native simplicity, helping you remove common deployment and operational barriers.

- **Deploy instantly** – Launch firewalls directly from the IBM Cloud catalog with no procurement or setup delays, using the same workflows as the rest of your cloud infrastructure.
- **Pay as you go** – Align costs to actual usage with a flexible OPEX model; billing is based on actual usage with no upfront commitment.
- **No license management** – Licensing is automatically applied at provisioning time. Eliminate renewals, tracking, and administrative overhead.
- **Scale on demand** – Adjust capacity and security features as workloads change.
- **Centralized visibility and control** – Monitor and control all network traffic entering, leaving, and moving within your VPC from a single management interface.
- **Comprehensive built-in security** – Protect workloads with IPS, application-aware policy enforcement, antivirus, web filtering, VPN, and continuous threat intelligence updates.

## Planning considerations
{: #getting-started-planning}

Review the following important considerations before you order:

- **License plan is permanent** — The license plan that you select at deployment cannot be changed after provisioning. You are billed for that license on the virtual server instance for as long as it runs. If you need a different license plan, you must place a new order and cancel the existing deployment. You can resize the virtual server instance without changing the license. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
- **Security group denies all inbound traffic by default** — A floating IP is automatically assigned to port1 (the public-facing interface) and is visible in your VPC resources immediately after deployment. However, you cannot connect to the firewall until you add an inbound security group rule that allows management access from your administrator IP address. For more information, see [Accessing the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).

## Getting started
{: #getting-started-next-steps}

Before you begin, ensure that the following resources exist in your IBM Cloud account:

- An IBM Cloud account with access to VPC
- A VPC with subnets configured for your deployment
- Appropriate IAM permissions to create and manage resources
- A secure access method (such as VPN or bastion host) to reach the firewall instance

_This third-party product is provided by a vendor outside of IBM and is subject to a separate agreement between you and the third party if you accept their terms. IBM is not responsible for the product and makes no privacy, security, performance, support, or other commitments regarding the product._

To get started, complete the following steps:

1. [Review license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles) — understand the available license tiers, deployment sizes, and performance characteristics before ordering.
1. [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — review the bootstrap configurations applied to each of the five deployment options (Single VM, HA single-zone active, HA single-zone passive, HA cross-zone active, HA cross-zone passive).
1. [Order a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-fortinet-firewall-order) — deploy a single VM, HA single zone, or HA cross zone configuration from the IBM Cloud catalog.
1. [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall) — add an inbound security group rule to allow management access, configure VPC routing to pass traffic through the firewall, and log in to the management interface for the first time. Step-by-step instructions for each of these tasks are provided in that topic.
1. [Monitor your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-monitoring) — review logs, check instance health, and verify that firewall policies are working as expected.

If you are migrating an existing Classic FortiGate deployment to VPC, see [Migrating Fortinet FortiGate from Classic to VPC PayGo](/docs/licensed-firewall?topic=licensed-firewall-tutorial-fortigate-vpc-migration).
{: note}
