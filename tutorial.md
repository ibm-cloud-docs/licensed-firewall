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
{: figure caption="High-level architecture for FortiGate migration from Classic infrastructure to VPC PayGo."}

**Workflow:**

1. Assess your current FortiGate Classic deployment
2. Design the target VPC architecture
3. Export and adapt configuration for VPC
4. Deploy a FortiGate VPC PayGo instance via Marketplace
5. Verify license activation
6. Restore configuration
7. Validate connectivity
8. Redirect traffic
9. Decommission Classic resources

---

## Before you begin
{: #fortigate-vpc-prereqs}

Ensure the following prerequisites:

* Access to your existing FortiGate Classic instance
* Backup/export of current configuration
* Understanding of network topology, firewall policies, and VPN configurations
* IBM Cloud VPC access and permissions
* Familiarity with IBM Cloud Marketplace deployments

> **Notes:**
> - IBM automatically applies FortiGate licenses when provisioning instances based on the selected VPC profile.
> - For detailed deployment guidance, refer to the [FortiGate Transit VPC patterns and deployment guide](/docs/pattern-transit-vpc-fortigate).

---

## Assess current deployment
{: #fortigate-vpc-step1}
{: step}

Inventory your environment:

- Interfaces and IP addresses
- Firewall policies and NAT rules
- VPN tunnels (IPsec/SSL)
- Routing configuration
- Throughput and license tier

**Outcome:** Define your target VPC architecture.

---

## Design target VPC architecture
{: #fortigate-vpc-step2}
{: step}

Map VLANs to VPC subnets and define:

- Public vs private subnets
- Availability zones
- Routing tables
- Floating IP usage and public gateway placement

Consider multi-zone design for high availability.
{: tip}

- **Reference patterns:** See [FortiGate Transit VPC patterns](https://cloud.ibm.com/docs/pattern-transit-vpc-fortigate) for guidance on architecture and deployment options.

---

## Export and adapt configuration
{: #fortigate-vpc-step3}
{: step}

- Export configuration from Classic FortiGate
- Update interface mappings, IP addresses/subnets, and gateway references

Hardcoded interface names or IPs will break in VPC.
{: note}

---

## Deploy FortiGate in VPC (PayGo)
{: #fortigate-vpc-step4}
{: step}

Deploy via IBM Cloud Marketplace:

1. Select Fortinet FortiGate offering
1. Choose PayGo license plan
1. Provide deployment inputs (Terraform-based)

Select a VPC profile that closely matches your Classic FortiGate performance (VCPU, memory, and bandwidth) to ensure a smooth cutover.
{: tip}

Behind the scenes:

- Software CRN (SWCRN) is created
- Platform License Manager requests the license
- VNF License Service interacts with FortiFlex to create the license
- Cloud-init retrieves license via Instance Metadata Service

Metadata service must be enabled.
{: important}

---

## Verify license activation
{: #fortigate-vpc-step5}
{: step}

- Log into FortiGate and confirm license status is valid
- Confirm correct entitlement/tier is applied
- No manual license upload is required

---

## Restore configuration
{: #fortigate-vpc-step6}
{: step}

- Import updated configuration into new FortiGate
- Validate interfaces, policies, NAT rules, and VPN tunnels

---

## Validate connectivity
{: #fortigate-vpc-step7}
{: step}

- Test internal traffic, external access, VPN connectivity, and failover behavior if HA is configured

---

## Redirect traffic to the VPC FortiGate
{: #fortigate-vpc-step8}
{: step}

When cutting over traffic from Classic to VPC:

- **Confirm licensing:** IBM automatically applies the FortiGate license per selected profile.
- **Apply configuration:** Ensure all firewall rules and routing policies are active. Customer maintains these settings.
- **Map profiles:** Match Classic FortiGate resources (VCPU, bandwidth) to VPC profiles. Use [Fortinet datasheets](https://www.fortinet.com/resources/datasheets){: external} or IBM Cloud guides.
- **Optional validation:** Run Classic and VPC instances in parallel to validate traffic and performance.
- **Update DNS/routing:** Redirect traffic to VPC FortiGate and monitor flows.

> **Reference:** See [FortiGate architectural patterns](/docs/pattern-transit-vpc-fortigate) for recommended deployment designs and best practices.

---

## Decommission Classic environment
{: #fortigate-vpc-step9}
{: step}

- Shut down Classic FortiGate instance
- Confirm no active traffic remains and billing has stopped

---

## Known limitations and considerations
{: #fortigate-vpc-limitations}

* 1
* 2
* 3

---

## Next steps
{: #fortigate-vpc-next}

- Test firewall rules and traffic in VPC environment
- Review FortiGate logs for licensing confirmation
- Plan ongoing VPC PayGo management and monitoring
