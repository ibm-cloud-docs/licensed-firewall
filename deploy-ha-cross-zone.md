---

copyright:
  years: 2026
lastupdated: "2026-08-12"

keywords: deploy firewall, FortiGate, HA cross zone, high availability, Terraform, Schematics, public address range

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a FortiGate HA Cross Zone firewall
{: #deploy-ha-cross-zone}

The HA Cross Zone offering deploys two FortiGate next-generation firewall instances as an active/passive (A/P) cluster across two separate availability zones. In addition to automatic failover via the IBM Cloud SDN connector, a public address range enables the cluster's floating IP address to move between zones, providing resilience against a full zone outage. This is the highest-availability topology and is recommended for production workloads with strict uptime requirements.
{: shortdesc}

## Before you begin
{: #deploy-ha-cross-zone-prereqs}

Before you deploy, ensure that you have reviewed [Planning for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-planning) and that the following resources are available in your IBM Cloud account:

- A VPC in the target region
- 8 subnets — four per zone, one for each port (`port1`–`port4`)
- A pre-created SSH key in the target region
- An IBM Cloud API key with sufficient permissions to create VPC resources

Two security groups are created automatically during deployment — one for the public interface and one for the private interface. You do not need to create them in advance.

This offering requires static IP addresses for all FortiGate interfaces across both zones. Allocate eight subnets — four per zone — and plan your IP assignments before you begin. Also verify that your account has sufficient floating IP quota in the target region, as four floating IPs are consumed.
{: important}

## Deploying an HA Cross Zone firewall
{: #deploy-ha-cross-zone-steps}

Terraform deploys the following resources:

- Two FortiGate licensed instances across two availability zones, each with four network interfaces (`port1`–`port4`)
- Four floating public IP addresses: one on `port1` and one on `port4` of each FortiGate
- One public address range for floating IP failover across zones
- One log disk per FortiGate
- Two security groups — one for the public interfaces (`port1` and `port4`) and one for the private interfaces (`port2` and `port3`) — with restrictive inbound rules (license download on the public group; HA heartbeat traffic on TCP/UDP port 703 on the private group) and allow-all outbound rules
- A bootstrap configuration with HA, SDN connector, public address range, and VDOM exception settings

Follow these steps:

1. Log in to the [IBM Cloud console](/login).
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate VM NGFW** and select the **Fortinet FortiGate VM Next-Generation Firewall - Cross Zone A/P HA** tile.
1. In **Select your deployment target**, select **IBM Cloud**.
1. In **Select a delivery method**, select **Terraform**.
1. In **Select product version**, choose a product version from the dropdown.
1. In **Select engine type**, select **Schematics**.
1. In **Configure your workspace**, review or update the following fields:
   - **Name** — A name for the Schematics workspace. A default name is pre-filled.
   - **Location** — The region where the Schematics workspace is created.
   - **Resource group** — The resource group for the workspace.
   - **Tags** — Optional tags to apply to the workspace.
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
   | `PAR_ADDRESS_COUNT` | The number of IPv4 addresses in the public address range. The public address range enables the floating IP to move between zones on failover. |
   | `REGION` | The IBM Cloud region where the firewall is deployed (for example, `us-east`). |
   | `RESOURCE_GRP` | The resource group name to attach to the FortiGate instances (default: `Default`). |
   | `SSH_PUBLIC_KEY_NAME` | The name or ID of your pre-created SSH public key in the target region. |
   | `SUBNET_1_Z1` | The ID of the primary, public subnet in zone 1 used for `port1` on the active FortiGate. |
   | `SUBNET_1_Z2` | The ID of the primary, public subnet in zone 2 used for `port1` on the passive FortiGate. |
   | `SUBNET_2_Z1` | The ID of the secondary, private subnet in zone 1 used for `port2` on the active FortiGate. |
   | `SUBNET_2_Z2` | The ID of the secondary, private subnet in zone 2 used for `port2` on the passive FortiGate. |
   | `SUBNET_3_Z1` | The ID of the subnet in zone 1 used for the HA heartbeat mechanism. Tied to `port3`. |
   | `SUBNET_3_Z2` | The ID of the subnet in zone 2 used for the HA heartbeat mechanism. Tied to `port3`. |
   | `SUBNET_4_Z1` | The ID of the subnet in zone 1 used for the HA management interface. Tied to `port4`. |
   | `SUBNET_4_Z2` | The ID of the subnet in zone 2 used for the HA management interface. Tied to `port4`. |
   | `VPC` | The name of the VPC where the FortiGate instances are deployed. |
   | `ZONE_1` | The first deployment zone within the region (for example, `us-east-1`). |
   | `ZONE_2` | The second deployment zone within the region (for example, `us-east-2`). |
   {: caption="HA Cross Zone required input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FGT1_Port1_Public_IP` — Public IP address for FortiGate 1 primary management (`port1`)
- `FGT1_Port4_Public_IP` — Public IP address for FortiGate 1 HA management (`port4`)
- `FGT2_Port1_Public_IP` — Public IP address for FortiGate 2 primary management (`port1`)
- `FGT2_Port4_Public_IP` — Public IP address for FortiGate 2 HA management (`port4`)
- `Par_ID` — Public address range ID
- `Par_CIDR` — Public address range CIDR block
- `Security_Group_ID` — ID of the automatically created security group
- `Security_Group_Name` — Name of the security group
- `Selected_VSI_Profile` — Virtual server instance profile automatically selected based on the plan CRN
- `Catalog_Offering_Version_CRN` — Catalog offering version CRN used
- `Catalog_Offering_Plan_CRN` — Catalog offering plan CRN used
- `Username` — Administrator username (`admin`)
- `FGT1_Default_Admin_Password` — Initial password for FortiGate 1. May be empty on initial startup; if so, use the instance ID as the initial password.
- `FGT2_Default_Admin_Password` — Initial password for FortiGate 2. May be empty on initial startup; if so, use the instance ID as the initial password.

Save these values before you close the workspace. When **Terraform commands successful** and **Cart creation successful** are both displayed, your HA cross-zone firewall pair is provisioned and ready to use.

It is a good idea to review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

## Next steps
{: #deploy-ha-cross-zone-next-steps}

- [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall) — Add a security group rule, configure routing, and log in for the first time.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — Review what IBM applied during provisioning before making changes.
