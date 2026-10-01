---

copyright:
  years: 2026
lastupdated: "2026-10-01"

keywords: deploy firewall, FortiGate, HA single zone, high availability, Terraform, Schematics

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a FortiGate HA Single Zone firewall
{: #deploy-ha-single-zone}

The HA Single Zone offering deploys two FortiGate VM next-generation firewall instances as an active/passive (A/P) cluster within a single availability zone. The IBM Cloud SDN connector monitors cluster health and triggers automatic failover if the active node becomes unavailable, with no manual intervention required. This topology is recommended for production workloads that require redundancy within a zone.
{: shortdesc}

## Before you begin
{: #deploy-ha-single-zone-prereqs}

Before you deploy, ensure that you review [Planning for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-planning) and that the following resources are available in your IBM Cloud account:

- A VPC in the target region
- 4 subnets in the VPC — public (`port1`), private (`port2`), HA heartbeat (`port3`), HA management (`port4`)
- A Public Gateway that is attached to the `port1` (public) subnet
- A pre-created SSH key in the target region
- An IBM Cloud API key with sufficient permissions to create VPC resources
- If a Network Access Control List (NACL) is applied to the public subnet, confirm that it includes egress rules that allow outbound traffic on UDP 53, TCP 443, and TCP 8890. These ports are required for FortiGate VM license registration and periodic FortiGuard validation.

Two security groups are created automatically during deployment, one for the public interface and one for the private interface. You do not need to create them in advance.

A Public Gateway must be attached to the `port1` subnet before deployment. The FortiGate VM instances use it to reach IBM Cloud licensing services during initial boot.
{: important}

This offering requires static IP addresses for all FortiGate VM interfaces. Allocate four subnets and plan your IP assignments before you begin.
{: important}

## Deploying an HA Single Zone firewall
{: #deploy-ha-single-zone-steps}

Terraform deploys the following resources:

- Two FortiGate VM licensed instances, each with four network interfaces (`port1`–`port4`)
- Three floating public IP addresses: one on the active node's `port1` (which fails over), and one each on `port4` (HA management) of both nodes
- One log disk per FortiGate VM
- Two security groups: one for the public interfaces (`port1` and `port4`) and one for the private interfaces (`port2` and `port3`) — each with restrictive inbound rules that allow the instance to download the license and enable cluster synchronization, including HA traffic on TCP/UDP port `703` from the HA heartbeat subnet, and allow-all outbound rules
- A bootstrap configuration with HA and SDN connector settings

Follow these steps:

1. Log in to the [IBM Cloud console](/login).
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate VM NGFW** and select the **Fortinet FortiGate VM NGFW - A/P HA (IBM reseller)** tile.
1. In **Select your deployment target**, select **IBM Cloud**.
1. In **Select a delivery method**, select **Terraform**.
1. In **Select product version**, choose a product version from the menu.
1. In **Select engine type**, select **Schematics**.
1. In **Configure your workspace**, review or update the following fields:
   - **Name**: A name for the Schematics workspace. A default name is pre-filled.
   - **Location**: The region where the Schematics workspace is created.
   - **Resource group**: The resource group for the workspace.
   - **Tags**: Optional tags to apply to the workspace.
1. Scroll down to **Set the input variables** and complete the fields in the **Required input variables** table:

   | Parameter | Description |
   |---|---|
   | `CATALOG_OFFERING_PLAN_CRN` | Click the field to open the plan selector. Select the plan that matches your required license tier (ATP, UTP, or Enterprise) and vCPU size. The cost per CPU hour is displayed for each option. |
   | `CLUSTER_NAME` | A name for your FortiGate HA deployment. Must be lowercase. A random suffix is appended automatically. |
   | `FGT1_PORT4_MGMT_GATEWAY` | Gateway for the HA management port (`port4`) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT1` | Static IP address for `port1` (public interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT2` | Static IP address for `port2` (private interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT3` | Static IP address for `port3` (HA heartbeat interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT4` | Static IP address for `port4` (HA management interface) on the primary (active) FortiGate. |
   | `FGT2_PORT4_MGMT_GATEWAY` | Gateway for the HA management port (`port4`) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT1` | Static IP address for `port1` (public interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT2` | Static IP address for `port2` (private interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT3` | Static IP address for `port3` (HA heartbeat interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT4` | Static IP address for `port4` (HA management interface) on the secondary (passive) FortiGate. |
   | `IBMCLOUD_API_KEY` | Your IBM Cloud API key. Required for the SDN connector for HA synchronization. |
   | `NETMASK` | Subnet mask for the static IP addresses and NICs of each FortiGate (default: `255.255.255.0`). |
   | `REGION` | The IBM Cloud region where the firewall is deployed (for example, `us-east`). |
   | `RESOURCE_GRP` | The resource group name to attach to the FortiGate instances (default: `Default`). |
   | `SSH_PUBLIC_KEY` | The name or ID of your pre-created SSH public key in the target region. |
   | `SUBNET_1` | The ID of the primary, public subnet used for `port1` on both FortiGate instances. |
   | `SUBNET_2` | The ID of the secondary, private subnet used for `port2` on both FortiGate instances. |
   | `SUBNET_3` | The ID of the subnet used for the HA heartbeat mechanism. Tied to `port3`. |
   | `PUBLIC_GATEWAY_ID` | The ID of the Public Gateway that is attached to the `port1` subnet. Required for the FortiGate instances to reach IBM Cloud licensing services during initial boot. |
   | `SUBNET_4` | The ID of the subnet used for the HA management interface. Tied to `port4`. |
   | `VPC` | The name of the VPC where the FortiGate instances are deployed. |
   | `ZONE` | The deployment zone within the region (for example, `us-east-1`). Only a single zone is supported for this topology. |
   {: caption="HA Single Zone required input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FortiGate_Public_IP`: Public IP address of the FortiGate cluster, which is attached to the active instance
- `FGT1_Public_HA_Mangment_IP`: Public IP address for FortiGate 1 HA management (`port4`)
- `FGT2_Public_HA_Mangment_IP`: Public IP address for FortiGate 2 HA management (`port4`)
- `Security_Group_ID`: ID of the automatically created security group
- `Security_Group_Name`: Name of the security group
- `Selected_VSI_Profile`: Virtual server instance profile automatically selected based on the plan CRN
- `Catalog_Offering_Version_CRN`: Catalog offering CRN version used
- `Catalog_Offering_Plan_CRN`: Catalog offering plan CRN used
- `Username`: Administrator username (`admin`)
- `FGT1_Default_Admin_Password`: Initial password for FortiGate 1. Might be empty on initial startup; if so, use the instance ID as the initial password.
- `FGT2_Default_Admin_Password`: Initial password for FortiGate 2. Might be empty on initial startup; if so, use the instance ID as the initial password.

Save these values before you close the workspace. When **Terraform commands successful** and **Cart creation successful** are both displayed, your HA firewall pair is provisioned and ready to use.

It is a good idea to review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

## Next steps
{: #deploy-ha-single-zone-next-steps}

- [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall): Add a security group rule, configure routing, and log in for the first time.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration): Review what IBM applied during provisioning before you make changes.
- [Understanding FortiGate licensing](/docs/licensed-firewall?topic=licensed-firewall-understanding-fortigate-licensing): Understand the Public Gateway requirement for the secondary node and how licensing is activated on initial startup.
