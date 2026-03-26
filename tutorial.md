---
copyright:
   years: 2026
lastupdated: "2026-03-26"

keywords: ibm cloud, fortinet, fortigate, firewall, migration, vpc, paygo, tutorial

subcollection: licensed-firewalls

content-type: tutorial
services: network, firewall, vpc
account-plan: paid
completion-time: 30m
---

{{site.data.keyword.attribute-definition-list}}

# Migrating Fortinet FortiGate from Classic to VPC PayGo
{: #tutorial-fortigate-vpc-migration}
{: toc-content-type="tutorial"}
{: toc-services="network, firewall, vpc"}
{: toc-completion-time="30m"}

In this tutorial, you learn how to migrate your Fortinet FortiGate deployment from IBM Cloud Classic to the new VPC Pay-As-You-Go (PayGo) licensed firewall offering. You will deploy a new VPC firewall, migrate your configuration, and validate licensing using IBM Cloud and Fortinet tools.
{: shortdesc}

![Architecture diagram](images/fortigate-vpc-arch.svg)
{: figure caption="High-level architecture for FortiGate migration from Classic nfrastructure to VPC PayGo."}

This workflow includes:

1. Assessing your current FortiGate Classic deployment
1. Designing the target VPC architecture
1. Exporting and adapting configuration for VPC
1. Deploying a FortiGate VPC PayGo instance via Marketplace
1. Verifying license activation
1. Restoring configuration
1. Validating connectivity
1. Cutting over traffic
1. Decommissioning Classic resources

## Before you begin
{: #fortigate-vpc-prereqs}

Before you begin, ensure the following prerequisites are met:

* Access to your existing FortiGate Classic instance
* Backup/export of current configuration
* Understanding of current network topology, firewall policies, and VPN configurations
* IBM Cloud VPC access and permissions
* Familiarity with IBM Cloud Marketplace deployments

## Assess current deployment
{: #fortigate-vpc-step1}
{: step}

Inventory your existing environment:

- Interfaces and IP addresses
- Firewall policies and NAT rules
- VPN tunnels (IPsec/SSL)
- Routing configuration
- Throughput and license tier

**Outcome:** Define your target VPC architecture.

## Design target VPC architecture
{: #fortigate-vpc-step2}
{: step}

Map VLANs to VPC subnets and define:

- Public vs private subnets
- Availability zones
- Routing tables
- Floating IP usage and public gateway placement

**Tip:** Consider multi-zone design for high availability.

## Export and Adapt configuration
{: #fortigate-vpc-step3}
{: step}

- Export configuration from existing FortiGate
- Update interface mappings, IP addresses/subnets, and gateway references

⚠️ Hardcoded interface names or IPs will break in VPC.

## Deploy FortiGate in VPC (PayGo)
{: #fortigate-vpc-step4}
{: step}

Deploy via IBM Cloud Marketplace:

1. Select Fortinet FortiGate offering
2. Choose PayGo license plan
3. Provide deployment inputs (Terraform-based)

Behind the scenes:

- Software CRN (SWCRN) created
- Platform License Manager requests license
- VNF License Service interacts with FortiFlex to create license
- Cloud-init retrieves license via Instance Metadata Service

Metadata service must be enabled.
{: important}

## Verify license activation
{: #fortigate-vpc-step5}
{: step}

- Log into FortiGate and confirm license status is valid
- Confirm correct entitlement/tier is applied
- No manual license upload required

## Restore configuration
{: #fortigate-vpc-step6}
{: step}

- Import updated configuration into new FortiGate
- Validate interfaces, policies, NAT rules, and VPN tunnels

## Validate connectivity
{: #fortigate-vpc-step7}
{: step}

Test internal traffic, external access, VPN connectivity, and failover behavior if HA is configured.

## Redirect traffic
{: #fortigate-vpc-step8}
{: step}

- Update DNS or routing as needed
- Consider running Classic and VPC environments in parallel during validation

## Decommission Classic environment
{: #fortigate-vpc-step9}
{: step}

- Shut down Classic FortiGate instance
- Ensure no active traffic and billing has stopped

## Known limitations and considerations
{: #fortigate-vpc-limitations}

 * 1
 * 2
 * 3

## Next steps
{: #fortigate-vpc-next}

- Test firewall rules and traffic in VPC environment
- Review FortiGate logs for licensing confirmation
- Plan for ongoing VPC PayGo management and monitoring
