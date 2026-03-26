---
copyright:
   years: 2026
lastupdated: "2026-03-26"

keywords: ibm cloud, fortinet, fortigate, firewall, migration, vpc, paygo, tutorial

subcollection: licensed-firewalls

content-type: tutorial
services: network, firewall, vpc
account-plan: paid
completion-time: 60m
---

{{site.data.keyword.attribute-definition-list}}

# Migrating Fortinet FortiGate from Classic to VPC PayGo
{: #tutorial-fortigate-vpc-migration}
{: toc-content-type="tutorial"}
{: toc-services="network, firewall, vpc"}
{: toc-completion-time="60m"}

In this tutorial, you learn how to migrate your Fortinet FortiGate deployment from IBM Cloud Classic to the new VPC Pay-As-You-Go (PayGo) licensed firewall offering. You will deploy a new VPC firewall, migrate your configuration, and validate licensing using IBM Cloud and Fortinet tools.
{: shortdesc}

![Architecture diagram](images/fortigate-vpc-arch.svg)
{: figure caption="High-level architecture for FortiGate migration from Classic infrastructure to VPC PayGo."}

Workflow:

1. Assess your current FortiGate Classic deployment
1. Design the target VPC architecture
1. Export and adapt configuration for VPC
1. Deploy a FortiGate VPC PayGo instance via Marketplace
1. Verify license activation
1. Restore configuration
1. Validate connectivity
1. Redirect traffic
1. Decommission Classic resources

---

## Before you begin
{: #fortigate-vpc-prereqs}

Ensure the following prerequisites are met before starting the migration:

- You have access to your existing FortiGate Classic instance.
- You have backed up or exported the current configuration.
- You understand your network topology, firewall policies, and VPN configurations.
- You have IBM Cloud VPC access and the necessary permissions.
- You are familiar with IBM Cloud Marketplace deployments.

**Notes:**
- IBM automatically applies FortiGate licenses when provisioning instances based on the selected VPC profile.
- For detailed deployment guidance, refer to the [FortiGate Transit VPC patterns and deployment guide](/docs/pattern-transit-vpc-fortigate).

---

## Assess current deployment
{: #fortigate-vpc-step1}
{: step}

Begin by gathering detailed information about your Classic FortiGate environment:

- List all network interfaces and assigned IP addresses.
- Document existing firewall rules and NAT configurations.
- Record VPN tunnel settings and encryption details.
- Note current static and dynamic routing rules.
- Capture performance metrics and license tier for migration planning.

**Outcome:** Define your target VPC architecture.

---

## Design target VPC architecture
{: #fortigate-vpc-step2}
{: step}

Plan your VPC network layout and map it to existing resources:

- Determine which subnets will host public-facing versus internal resources.
- Select availability zones to support high availability and redundancy.
- Configure routing for inter-subnet and external connectivity.
- Plan floating IPs and gateways for internet access.

Consider multi-zone design for high availability.
{: tip}

- Reference patterns in the [FortiGate Transit VPC patterns](/docs/pattern-transit-vpc-fortigate) guide for architecture and deployment options.

---

## Export and adapt configuration
{: #fortigate-vpc-step3}
{: step}

Prepare your Classic FortiGate configuration for VPC deployment:

- Export the configuration from the Classic FortiGate.
- Update interface mappings, IP addresses/subnets, and gateway references to match the VPC.

Hardcoded interface names or IPs will break in VPC.
{: note}

---

## Deploy FortiGate in VPC (PayGo)
{: #fortigate-vpc-step4}
{: step}

Follow these steps to deploy the FortiGate VPC instance via IBM Cloud Marketplace:

1. Choose the Fortinet FortiGate offering for VPC PayGo.
1. Pick the PayGo license plan that fits your deployment.
1. Provide deployment inputs, including network, credentials, and resource details.

Select a VPC profile that closely matches your Classic FortiGate performance (VCPU, memory, and bandwidth) to ensure a smooth cutover.
{: tip}

Behind the scenes, the system handles licensing and provisioning:

- A Software CRN (SWCRN) is created for the deployment.
- Platform License Manager requests the FortiGate license automatically.
- VNF License Service interacts with FortiFlex to provision the license.
- Cloud-init retrieves the license via the Instance Metadata Service.

Metadata service must be enabled.
{: important}

---

## Verify license activation
{: #fortigate-vpc-step5}
{: step}

After deployment, confirm that licensing is correctly applied:

- Log into the FortiGate administrative console to check license status.
- Verify that the license matches your VPC profile entitlement.
- No manual license upload is required as IBM handles this automatically.

---

## Restore configuration
{: #fortigate-vpc-step6}
{: step}

Import and validate your configuration in the VPC environment:

- Import the adapted configuration file into the VPC instance.
- Validate that interfaces, policies, NAT rules, and VPN tunnels function correctly.

---

## Validate connectivity
{: #fortigate-vpc-step7}
{: step}

Test the network and security connectivity to ensure a functional deployment:

- Ensure internal resources can communicate within the VPC.
- Verify external access and internet connectivity.
- Check VPN tunnels for proper routing and encryption.
- Test failover behavior if HA is configured.

---

## Redirect traffic to the VPC FortiGate
{: #fortigate-vpc-step8}
{: step}

Cut over traffic from Classic to VPC carefully to avoid downtime:

- Confirm that the FortiGate license is active; IBM automatically applies it based on the selected profile.
- Apply all firewall rules and routing policies on the VPC FortiGate; customer remains responsible for these settings.
- Map Classic FortiGate resources (VCPU, bandwidth) to the appropriate VPC profile. Refer to [Fortinet datasheets](https://www.fortinet.com/resources/datasheets){: external} or IBM Cloud guides as needed.
- Optionally, run Classic and VPC instances in parallel to validate traffic and performance.
- Update DNS or routing to direct traffic to the VPC FortiGate and monitor flows.

> Reference: See [FortiGate architectural patterns](/docs/pattern-transit-vpc-fortigate) for recommended deployment designs and best practices.

---

## Decommission Classic environment
{: #fortigate-vpc-step9}
{: step}

Shut down your old Classic environment safely:

- Power off the Classic FortiGate instance.
- Confirm that no active traffic remains and that billing has stopped.

---

## Known limitations and considerations
{: #fortigate-vpc-limitations}

- 1
- 2
- 3

---

## Next steps
{: #fortigate-vpc-next}

Plan for ongoing operations and monitoring:

- Test firewall rules and traffic in VPC environment. Ensure all policies work as expected.
- Review FortiGate logs for licensing confirmation. Verify licenses remain active and compliant.
- Plan ongoing VPC PayGo management and monitoring. Set up monitoring, alerting, and operational procedures.
