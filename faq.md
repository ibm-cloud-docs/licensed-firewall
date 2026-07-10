---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate FAQ, licensed firewall questions, license plan, FortiGate VPC, firewall billing, resize firewall, HA firewall

subcollection: licensed-firewall

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for FortiGate licensed firewall
{: #my-service-faq}

Frequently asked questions for the FortiGate licensed firewall on IBM Cloud VPC. To find all of the FAQs for {{site.data.keyword.cloud}}, see our [FAQ library](/docs/faqs).
{: shortdesc}

## Can I change my license plan after deployment?
{: #faq-change-license}
{: faq}

No. The license plan is fixed at deployment time and cannot be changed on an existing instance. To use a different license plan, you must place a new order with the required plan, migrate your configuration to the new instance, and cancel the existing deployment. You are billed for both instances until the existing one is cancelled. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).

## Can I resize the virtual server instance without changing the license?
{: #faq-resize-vsi}
{: faq}

Yes. You can resize the underlying virtual server instance to a different profile without changing the license plan. The license and associated billing remain unchanged after a resize. You must stop the instance before resizing it. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).

## Why can't I connect to my FortiGate after deployment?
{: #faq-cannot-connect}
{: faq}

The security group created during deployment denies all inbound traffic by default. You must add an inbound TCP rule for port 443 (HTTPS) or port 22 (SSH) that allows your administrator IP address before you can connect to the FortiGate web console. For more information, see [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).

## How is the FortiGate license applied?
{: #faq-license-applied}
{: faq}

IBM applies the FortiGate license automatically when your instance is provisioned. You do not need to upload or activate a license manually. The license is retrieved by cloud-init through the Instance Metadata Service during startup. To verify that the license is active, log in to the FortiGate web console and check the license status under **System > FortiGuard**.

## What deployment configurations are available?
{: #faq-deployment-configs}
{: faq}

Three configurations are available from the IBM Cloud catalog: a single virtual machine (VM), a high-availability (HA) pair in a single zone, and an HA pair across two zones. All three are deployed by using IBM Cloud Schematics with Terraform automation. For more information, see [Deploying a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-single-vm).

## Who is responsible for applying FortiGate software updates and security patches?
{: #faq-updates-responsibility}
{: faq}

You are responsible for applying FortiGate firmware updates and security patches to your virtual server instances. IBM notifies license holders of critical security advisories from the Fortinet PSIRT and coordinates with Fortinet TAC when needed, but does not modify customer FortiGate configurations. For more information, see [Upgrading the FortiGate software](/docs/licensed-firewall?topic=licensed-firewall-upgrading-fortigate-software) and [Subscribing to Fortinet notifications](/docs/licensed-firewall?topic=licensed-firewall-security-maintenance-vulnerability-management).

## How do I open a support case for a FortiGate issue?
{: #faq-open-support-case}
{: faq}

Open all FortiGate support cases with IBM Support. IBM Support performs initial triage and opens a Fortinet Technical Assistance Center (TAC) case on your behalf when needed. Do not open TAC cases directly with Fortinet. For more information, including what details to include in your case, see [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support).

## Is the FortiGate licensed firewall a managed service?
{: #faq-managed-service}
{: faq}

No. IBM provides license management and support coordination, but the FortiGate licensed firewall is a customer-managed service. You are responsible for deploying, configuring, operating, and maintaining your FortiGate virtual server instances. IBM does not configure or manage your firewall policies or network settings.
