---

copyright:
  years: 2026

lastupdated: "2026-10-08"

keywords: FortiGate FAQ, licensed firewall questions, license plan, FortiGate VPC, firewall billing, resize firewall, HA firewall

subcollection: licensed-firewall

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ for FortiGate-VM licensed firewall
{: #my-service-faq}

Frequently asked questions for the FortiGate licensed firewall on IBM Cloud VPC.
{: shortdesc}

## Where can I find pricing information?
{: #faq-pricing}
{: faq}

Pricing information for licensed firewall offerings is coming soon. For the latest details, see [License plans, profiles, and pricing](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles#pricing).

## Can I change my license plan after deployment?
{: #faq-change-license}
{: faq}

No. The license plan is fixed at deployment time and cannot be changed on an existing instance. To use a different license plan, you must place a new order with the required plan, migrate your configuration to the new instance, and cancel the existing deployment. You are billed for both instances until the existing one is canceled. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).

## Can I resize the virtual server instance without changing the license?
{: #faq-resize-vsi}
{: faq}

Yes. You can resize the underlying virtual server instance to a different profile without changing the license plan. The license and associated billing remain unchanged after a resize. Stop the instance before you resize it. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).

## How is the FortiGate-VM license applied?
{: #faq-license-applied}
{: faq}

IBM applies the FortiGate-VM license automatically when your instance is provisioned. You do not need to upload or activate a license manually. The license is retrieved by `cloud-init` through the Instance Metadata Service during startup. To verify that the license is active, log in to the FortiGate web console and check the license status under **System > FortiGuard**. For information on how to access the FortiGate web console, see [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).

## What deployment configurations are available?
{: #faq-deployment-configs}
{: faq}

Three FortiGate PayGo catalog entries are available from the IBM Cloud catalog: a single virtual machine (VM), a high-availability (HA) pair in a single zone, and an HA pair across two zones. All three are Terraform-based deployable architectures that IBM deploys on your behalf through IBM Cloud Schematics. For more information about deploying through the catalog, see [Deploying a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-single-vm).



## Who is responsible for applying FortiGate-VM software updates and security patches?
{: #faq-updates-responsibility}
{: faq}

You are responsible for applying FortiGate-VM firmware updates and security patches to your virtual server instances. IBM notifies license holders of critical security advisories from the Fortinet PSIRT and coordinates with Fortinet TAC when needed, but does not modify customer FortiGate-VM configurations. For more information, see [Keeping abreast of firmware updates and vulnerability patches](/docs/licensed-firewall?topic=licensed-firewall-addressing-vulnerabilities).

## How do I open a support case for a FortiGate-VM issue?
{: #faq-open-support-case}
{: faq}

Open all FortiGate-VM support cases with IBM Support. IBM Support performs initial triage and opens a Fortinet Technical Assistance Center (TAC) case on your behalf when needed. Do not open TAC cases directly with Fortinet. For more information, including what details to include in your case, see [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support).

## Is the FortiGate-VM licensed firewall a managed service?
{: #faq-managed-service}
{: faq}

No. IBM provides license management and support coordination, but the FortiGate-VM licensed firewall is a customer-managed service. You are responsible for deploying, configuring, operating, and maintaining your FortiGate-VM instances. IBM does not configure or manage your firewall policies or network settings.

## Can I use IBM Cloud VPC snapshots to back up or restore my FortiGate-VM firewall?
{: #faq-vpc-snapshots}
{: faq}

No. VPC boot volume snapshots and whole-volume restores are not supported for FortiGate licensed firewall instances. Restoring a snapshot carries over the previous instance's FortiFlex license registration, which FortiOS cannot automatically refresh on the new instance, and bypasses the `cloud-init` and Terraform automation required to provision VPC networking resources and licensing properly. To back up and restore your firewall, export and import the FortiGate `.conf` configuration file. For more information, see [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config#unsupported-backup-methods).
