---

copyright:
  years: 2026
lastupdated: "2026-10-05"

keywords: FortiGate planning, licensed firewall planning, FortiGate limitations, security group, Fortinet notifications

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Planning for FortiGate on IBM Cloud VPC
{: #planning}

Before you deploy a FortiGate VM firewall, review the following topics and considerations to ensure that your configuration meets your security, compliance, and operational requirements.
{: shortdesc}

## Before you deploy
{: #planning-before-you-deploy}

Ensure that the following resources exist in your IBM Cloud account before you deploy:

- An IBM Cloud account with access to VPC
- A VPC with subnets configured for your deployment
- Appropriate IAM permissions to create and manage resources
- A secure access method (such as VPN or bastion host) to reach the firewall instance
- If a Network Access Control List (NACL) is applied to the public subnet, confirm that it includes egress rules that allow outbound traffic on UDP 53, TCP 443, and TCP 8890. These ports are required for FortiGate VM license registration and periodic FortiGuard validation.

Review the following topics before you deploy. Understanding your options upfront helps you avoid configuration decisions after deployment that require redeployment.

- [Review license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles) — Understand the available license tiers, deployment sizes, and performance characteristics.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — Review the bootstrap configurations that are applied to each of the five deployment options (Single VM, HA single-zone active, HA single-zone passive, HA cross-zone active, HA cross-zone passive).
-  — If you plan to deploy programmatically or integrate deployments into a pipeline, review this topic before you deploy manually.

## Planning considerations
{: #planning-considerations}

Review the following important considerations before you order:

- **License plan is permanent**: The license plan that you select at deployment cannot be changed after provisioning. You are billed for that license on the virtual server instance for as long as it runs. If you need a different license plan, you must place a new order and cancel the existing deployment. You can resize the virtual server instance without changing the license. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
- **Security groups deny all inbound management traffic**: A floating IP is automatically assigned to `port1` and is visible in your VPC resources immediately after deployment, but all inbound traffic to it is blocked. Two security groups are created automatically — one for the public interface and one for the private interface. Both include restrictive pre-configured inbound rules that allow the instance to download the license and enable cluster synchronization. They do not permit management access. You cannot connect to the firewall until you add an inbound rule that allows HTTPS or SSH from your administrator IP address. For more information, see [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).
- **Subscribe to Fortinet security notifications**: Stay informed about vulnerabilities, patches, and firmware updates. For more information, see [Keeping abreast of firmware updates and vulnerability patches](/docs/licensed-firewall?topic=licensed-firewall-addressing-vulnerabilities). Keeping your FortiGate VM firmware up to date is your responsibility and is essential to maintaining a secure posture.
- **Deployment size affects throughput and session capacity**: Larger deployment sizes provide higher throughput and support more concurrent sessions. Select a size that matches your expected traffic volume and workload.
- **Some configurations might not suit all architectures**: Certain instance configurations might be less suitable for high availability or hub-and-spoke architectures. Review your network design requirements before you select a deployment topology.
- **All deployments include FortiCare Premium support**: FortiCare Premium support is included with every license plan at no additional cost.

## Limitations
{: #planning-limitations}

The following limitations apply to this offering. Review them before you deploy.

- **Regional availability**: All license plans, including ATP, are available in all supported regions.
- **VPC boot volume snapshots are not supported**: You cannot use IBM Cloud VPC volume snapshots or whole-volume backups to back up or restore FortiGate VM instances. Restoring from a boot volume snapshot creates licensing conflicts with FortiFlex and bypasses required deployment automation. Use FortiGate configuration file (`.conf`) export and import instead. For more information, see [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config#unsupported-backup-methods).
- **FortiGate Cloud and FortiCloud access is not available**: Because the license is managed through the IBM Fortinet account, access to FortiGate Cloud and FortiCloud is not supported. AI-based inline malware prevention (Enterprise license) can be configured and used locally but cannot connect to FortiGate Cloud services.
- **This is not a managed service**: IBM manages licensing and provides support coordination, but you are responsible for deploying, configuring, and maintaining your FortiGate VM instances. IBM does not configure firewall policies or apply updates on your behalf. For more information, see [Shared responsibilities](/docs/licensed-firewall?topic=licensed-firewall-shared-responsibilities).
