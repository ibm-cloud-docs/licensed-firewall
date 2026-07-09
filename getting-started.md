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

- **Deploy instantly** – Launch firewalls directly from the catalog with no procurement or setup delays
- **Pay as you go** – Align costs to actual usage with a flexible OPEX model
- **No license management** – Eliminate renewals, tracking, and administrative overhead
- **Scale on demand** – Adjust capacity and security features as workloads change
- **Comprehensive built-in security** – Protect workloads with integrated threat detection, application-aware policies, and services such as antivirus, web filtering, VPN, and continuous threat intelligence

## Highlights of FortiGate on IBM Cloud VPC
{: #fortigate-features}

FortiGate delivers core security capabilities that are critical for protecting workloads and maintaining strong security in IBM Cloud VPC environments.

* Provides centralized visibility and control over cloud network traffic
* Delivers integrated protection for network traffic and application content
* Uses IPS technology to detect and block known and emerging threats
* Enables application-aware policy enforcement for more granular security control
* Includes built-in security services such as antivirus, web filtering, and VPN access
* Leverages continuous threat intelligence updates to address evolving attacks

You get consistent, end-to-end protection across your VPC without needing to integrate multiple security tools.

### How it works
{: #how-it-works}

FortiGate is deployed as a virtual firewall inside your VPC and integrates with IBM Cloud services:

* Deployed directly from the IBM Cloud catalog
* Licensing is automatically applied through the pay-as-you-go model
* Billing is based on actual usage
* Managed alongside your other VPC resources

You can deploy and operate enterprise firewall security using the same workflows as the rest of your cloud infrastructure.

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

If you are migrating an existing Classic FortiGate deployment to VPC, see [Migrating Fortinet FortiGate from Classic to VPC PayGo](/docs/licensed-firewall?topic=licensed-firewall-tutorial-fortigate-vpc-migration).
{: note}
