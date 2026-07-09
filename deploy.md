---

copyright:
  years: 2026

lastupdated: "2026-07-15"

keywords: deploy firewall, FortiGate, single VM, HA single zone, HA cross zone, Terraform, Schematics

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Ordering a licensed FortiGate firewall
{: #fortinet-firewall-order}

You can order a licensed Fortinet FortiGate Next-Generation Firewall from the IBM Cloud catalog in three configurations: a single virtual machine (VM), a high-availability (HA) pair in a single zone, or an HA pair across two zones. All three offerings are deployed by using IBM Cloud Schematics, which runs the Terraform automation on your behalf.
{: shortdesc}

## Before you begin
{: #deploy-fortinet-prereqs}

Before you deploy a FortiGate firewall, ensure that the following resources exist in your IBM Cloud account and that you have reviewed the [planning considerations](/docs/licensed-firewall?topic=licensed-firewall-getting-started#getting-started-planning).

The number of subnets required depends on the topology you are deploying:

| Topology | Subnets required |
|---|---|
| Single VM | 2 — one public (port1), one private (port2) |
| HA Single Zone | 4 — public (port1), private (port2), HA heartbeat (port3), HA management (port4) |
| HA Cross Zone | 8 — four per zone, one for each port |
{: caption="Subnet requirements by topology" caption-side="bottom"}

The security group is created automatically during deployment. You do not need to create one in advance.

HA Cross Zone deployments consume 4 floating IPs. Verify that your account has sufficient floating IP quota in the target region before deploying.
{: note}

- A VPC in the target region
- Subnets in the VPC as required for your topology (see table above)
- A pre-created SSH key in the target region
- An IBM Cloud API key with sufficient permissions to create VPC resources

## Deploying a FortiGate Single VM firewall
{: #deploy-fortigate-single-vm}

The Single VM offering deploys one FortiGate next-generation firewall virtual machine into your VPC. It provides a single public interface (port1) and a single private interface (port2) with no redundancy. This topology is suited for development, testing, or workloads where a brief interruption during instance recovery is acceptable.

Terraform deploys the following resources:

- One FortiGate licensed instance with two network interfaces (port1 and port2)
- One floating public IP address attached to port1
- One log disk
- One security group with deny-all inbound and allow-all outbound rules
- A bootstrap configuration

1. Log in to the [IBM Cloud console](https://cloud.ibm.com){: external}.
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate VM NGFW** and select the **Fortinet FortiGate VM Next-Generation Firewall - Single** tile.
1. Under **Configure your workspace**, review or update the following fields:
   - **Name** — a name for the Schematics workspace. A default name is pre-filled.
   - **Location** — the region where the Schematics workspace is created.
   - **Resource group** — the resource group for the workspace.
   - **Tags** — optional tags to apply to the workspace.
1. Scroll down to **Set the input variables** and complete the fields in the **Required input variables** table:

   | Parameter | Description |
   |---|---|
   | `CATALOG_OFFERING_PLAN_CRN` | Click the field to open the plan selector. Select the plan that matches your required license tier (ATP, UTP, or Enterprise) and vCPU size. The cost per CPU hour is displayed for each option. |
   | `CLUSTER_NAME` | A name for your FortiGate deployment. A random suffix is appended automatically to prevent naming collisions. |
   | `IBMCLOUD_API_KEY` | Your IBM Cloud API key. |
   | `REGION` | The IBM Cloud region where the firewall is deployed (for example, `us-south`). |
   | `SSH_PUBLIC_KEY` | The name of a pre-created SSH key in the target region. |
   | `SUBNET1` | The ID of the primary, public subnet used for port1 on the FortiGate. |
   | `SUBNET2` | The ID of the secondary, private subnet used for port2 on the FortiGate. |
   | `VPC` | The name of the VPC where the FortiGate is deployed. |
   | `ZONE_1` | The deployment zone within the region (for example, `us-south-1`). |
   | `RESOURCE_GROUP` | The resource group name to attach to the FortiGate instance. |
   {: caption="Single VM required input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FortiGate_Public_IP` — the public IP address of the FortiGate instance
- `Security_Group_ID` — the ID of the automatically created security group
- `Security_Group_Name` — the name of the security group
- `selected_vsi_profile` — the VSI profile automatically selected based on the plan CRN
- `CATALOG_OFFERING_VERSION_CRN` — the catalog offering version CRN used
- `CATALOG_OFFERING_PLAN_CRN` — the catalog offering plan CRN used
- `Username` — the administrator username (`admin`)
- `Default_Admin_Password` — the initial administrator password. The password may be empty on first boot; if so, use the instance ID as the initial password.

Save these values before you close the workspace — you need them to log in to the FortiGate web console for the first time. When **Terraform commands successful** and **Cart creation successful** are both displayed, your firewall is provisioned and ready to use.

It is a good idea to review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

Before you attempt to connect, review the [planning considerations](/docs/licensed-firewall?topic=licensed-firewall-getting-started#getting-started-planning) for information about the default security group posture and license plan constraints.
{: important}

## Deploying a FortiGate HA Single Zone firewall
{: #deploy-fortigate-ha-single-zone}

The HA Single Zone offering deploys two FortiGate next-generation firewall instances as an active/passive (A/P) cluster within a single availability zone. The IBM Cloud SDN connector monitors cluster health and triggers automatic failover if the active node becomes unavailable, with no manual intervention required. This topology is recommended for production workloads that require redundancy within a zone.

Terraform deploys the following resources:

- Two FortiGate licensed instances, each with four network interfaces (port1–port4)
- Three floating public IP addresses: one on the active node's port1 (which fails over), and one each on port4 (HA management) of both nodes
- One log disk per FortiGate
- One security group with inbound rules allowing HA traffic on TCP/UDP port 703 from the HA heartbeat subnet, and allow-all outbound rules
- A bootstrap configuration with HA and SDN connector settings

This offering requires static IP addresses for all FortiGate interfaces. Allocate four subnets and plan your IP assignments before you begin.
{: important}

1. Log in to the [IBM Cloud console](https://cloud.ibm.com){: external}.
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate VM NGFW** and select the **Fortinet FortiGate VM Next-Generation Firewall - A/P HA** tile.
1. Under **Select your deployment target**, select **IBM Cloud**.
1. Under **Select a delivery method**, select **Terraform**.
1. Under **Select product version**, choose a product version from the dropdown.
1. Under **Select engine type**, select **Schematics**.
1. Under **Configure your workspace**, review or update the following fields:
   - **Name** — a name for the Schematics workspace. A default name is pre-filled.
   - **Location** — the region where the Schematics workspace is created.
   - **Resource group** — the resource group for the workspace.
   - **Tags** — optional tags to apply to the workspace.
1. Scroll down to **Set the input variables** and complete the fields in the **Required input variables** table:

   | Parameter | Description |
   |---|---|
   | `CATALOG_OFFERING_PLAN_CRN` | Click the field to open the plan selector. Select the plan that matches your required license tier (ATP, UTP, or Enterprise) and vCPU size. The cost per CPU hour is displayed for each option. |
   | `CLUSTER_NAME` | A name for your FortiGate HA deployment. Must be lowercase. A random suffix is appended automatically. |
   | `FGT1_PORT4_MGMT_GATEWAY` | Gateway for the HA management port (port4) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT1` | Static IP address for port1 (public interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT2` | Static IP address for port2 (private interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT3` | Static IP address for port3 (HA heartbeat interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT4` | Static IP address for port4 (HA management interface) on the primary (active) FortiGate. |
   | `FGT2_PORT4_MGMT_GATEWAY` | Gateway for the HA management port (port4) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT1` | Static IP address for port1 (public interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT2` | Static IP address for port2 (private interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT3` | Static IP address for port3 (HA heartbeat interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT4` | Static IP address for port4 (HA management interface) on the secondary (passive) FortiGate. |
   | `IBMCLOUD_API_KEY` | Your IBM Cloud API key. Required for the SDN connector for HA synchronization. |
   | `NETMASK` | Subnet mask for the static IP addresses and NICs of each FortiGate (default: `255.255.255.0`). |
   | `REGION` | The IBM Cloud region where the firewall is deployed (for example, `us-east`). |
   | `RESOURCE_GRP` | The resource group name to attach to the FortiGate instances (default: `Default`). |
   | `SSH_PUBLIC_KEY` | The name or ID of your pre-created SSH public key in the target region. |
   | `SUBNET_1` | The ID of the primary, public subnet used for port1 on both FortiGate instances. |
   | `SUBNET_2` | The ID of the secondary, private subnet used for port2 on both FortiGate instances. |
   | `SUBNET_3` | The ID of the subnet used for the HA heartbeat mechanism. Tied to port3. |
   | `SUBNET_4` | The ID of the subnet used for the HA management interface. Tied to port4. |
   | `VPC` | The name of the VPC where the FortiGate instances are deployed. |
   | `ZONE` | The deployment zone within the region (for example, `us-east-1`). Only a single zone is supported for this topology. |
   {: caption="HA Single Zone required input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FortiGate_Public_IP` — the public IP address of the FortiGate cluster, attached to the active instance
- `FGT1_Public_HA_Mangment_IP` — the public IP address for FortiGate 1 HA management (port4)
- `FGT2_Public_HA_Mangment_IP` — the public IP address for FortiGate 2 HA management (port4)
- `Security_Group_ID` — the ID of the automatically created security group
- `Security_Group_Name` — the name of the security group
- `Selected_VSI_Profile` — the VSI profile automatically selected based on the plan CRN
- `Catalog_Offering_Version_CRN` — the catalog offering version CRN used
- `Catalog_Offering_Plan_CRN` — the catalog offering plan CRN used
- `Username` — the administrator username (`admin`)
- `FGT1_Default_Admin_Password` — the initial password for FortiGate 1. The password may be empty on first boot; if so, use the instance ID as the initial password.
- `FGT2_Default_Admin_Password` — the initial password for FortiGate 2. The password may be empty on first boot; if so, use the instance ID as the initial password.

Save these values before you close the workspace. When **Terraform commands successful** and **Cart creation successful** are both displayed, your HA firewall pair is provisioned and ready to use.

It is a good idea to review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

Before you attempt to connect, review the [planning considerations](/docs/licensed-firewall?topic=licensed-firewall-getting-started#getting-started-planning) for information about the default security group posture and license plan constraints.
{: important}

## Deploying a FortiGate HA Cross Zone firewall
{: #deploy-fortigate-ha-cross-zone}

The HA Cross Zone offering deploys two FortiGate next-generation firewall instances as an active/passive (A/P) cluster across two separate availability zones. In addition to automatic failover via the IBM Cloud SDN connector, a Public Address Range (PAR) enables the cluster's floating IP address to move between zones, providing resilience against a full zone outage. This is the highest-availability topology and is recommended for production workloads with strict uptime requirements.

Terraform deploys the following resources:

- Two FortiGate licensed instances across two availability zones, each with four network interfaces (port1–port4)
- Four floating public IP addresses: one on port1 and one on port4 of each FortiGate
- One Public Address Range (PAR) for floating IP failover across zones
- One log disk per FortiGate
- One security group with inbound rules allowing HA traffic from each FortiGate's port3 IP, and allow-all outbound rules
- A bootstrap configuration with HA, SDN connector, PAR, and VDOM exception settings

This offering requires static IP addresses for all FortiGate interfaces across both zones. Allocate eight subnets — four per zone — and plan your IP assignments before you begin.
{: important}

1. Log in to the [IBM Cloud console](https://cloud.ibm.com){: external}.
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate VM NGFW** and select the **Fortinet FortiGate VM Next-Generation Firewall - Cross Zone A/P HA** tile.
1. Under **Select your deployment target**, select **IBM Cloud**.
1. Under **Select a delivery method**, select **Terraform**.
1. Under **Select product version**, choose a product version from the dropdown.
1. Under **Select engine type**, select **Schematics**.
1. Under **Configure your workspace**, review or update the following fields:
   - **Name** — a name for the Schematics workspace. A default name is pre-filled.
   - **Location** — the region where the Schematics workspace is created.
   - **Resource group** — the resource group for the workspace.
   - **Tags** — optional tags to apply to the workspace.
1. Scroll down to **Set the input variables** and complete the fields in the **Required input variables** table:

   | Parameter | Description |
   |---|---|
   | `CATALOG_OFFERING_PLAN_CRN` | Click the field to open the plan selector. Select the plan that matches your required license tier (ATP, UTP, or Enterprise) and vCPU size. The cost per CPU hour is displayed for each option. |
   | `CLUSTER_NAME` | A name for your FortiGate HA deployment. Must be lowercase. A random suffix is appended automatically. |
   | `FGT1_PORT4_MGMT_GATEWAY` | Gateway for the HA management port (port4) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT1` | Static IP address for port1 (public interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT2` | Static IP address for port2 (private interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT3` | Static IP address for port3 (HA heartbeat interface) on the primary (active) FortiGate. |
   | `FGT1_STATIC_IP_PORT4` | Static IP address for port4 (HA management interface) on the primary (active) FortiGate. |
   | `FGT2_PORT4_MGMT_GATEWAY` | Gateway for the HA management port (port4) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT1` | Static IP address for port1 (public interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT2` | Static IP address for port2 (private interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT3` | Static IP address for port3 (HA heartbeat interface) on the secondary (passive) FortiGate. |
   | `FGT2_STATIC_IP_PORT4` | Static IP address for port4 (HA management interface) on the secondary (passive) FortiGate. |
   | `IBMCLOUD_API_KEY` | Your IBM Cloud API key. Required for the SDN connector for HA synchronization. |
   | `NETMASK` | Subnet mask for the static IP addresses and NICs of each FortiGate (default: `255.255.255.0`). |
   | `PAR_ADDRESS_COUNT` | The number of IPv4 addresses in the Public Address Range (PAR). The PAR enables the floating IP to move between zones on failover. |
   | `REGION` | The IBM Cloud region where the firewall is deployed (for example, `us-east`). |
   | `RESOURCE_GRP` | The resource group name to attach to the FortiGate instances (default: `Default`). |
   | `SSH_PUBLIC_KEY_NAME` | The name or ID of your pre-created SSH public key in the target region. |
   | `SUBNET_1_Z1` | The ID of the primary, public subnet in zone 1 used for port1 on the active FortiGate. |
   | `SUBNET_1_Z2` | The ID of the primary, public subnet in zone 2 used for port1 on the passive FortiGate. |
   | `SUBNET_2_Z1` | The ID of the secondary, private subnet in zone 1 used for port2 on the active FortiGate. |
   | `SUBNET_2_Z2` | The ID of the secondary, private subnet in zone 2 used for port2 on the passive FortiGate. |
   | `SUBNET_3_Z1` | The ID of the subnet in zone 1 used for the HA heartbeat mechanism. Tied to port3. |
   | `SUBNET_3_Z2` | The ID of the subnet in zone 2 used for the HA heartbeat mechanism. Tied to port3. |
   | `SUBNET_4_Z1` | The ID of the subnet in zone 1 used for the HA management interface. Tied to port4. |
   | `SUBNET_4_Z2` | The ID of the subnet in zone 2 used for the HA management interface. Tied to port4. |
   | `VPC` | The name of the VPC where the FortiGate instances are deployed. |
   | `ZONE_1` | The first deployment zone within the region (for example, `us-east-1`). |
   | `ZONE_2` | The second deployment zone within the region (for example, `us-east-2`). |
   {: caption="HA Cross Zone required input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FGT1_Port1_Public_IP` — the public IP address for FortiGate 1 primary management (port1)
- `FGT1_Port4_Public_IP` — the public IP address for FortiGate 1 HA management (port4)
- `FGT2_Port1_Public_IP` — the public IP address for FortiGate 2 primary management (port1)
- `FGT2_Port4_Public_IP` — the public IP address for FortiGate 2 HA management (port4)
- `Par_ID` — the Public Address Range ID
- `Par_CIDR` — the Public Address Range CIDR block
- `Security_Group_ID` — the ID of the automatically created security group
- `Security_Group_Name` — the name of the security group
- `Selected_VSI_Profile` — the VSI profile automatically selected based on the plan CRN
- `Catalog_Offering_Version_CRN` — the catalog offering version CRN used
- `Catalog_Offering_Plan_CRN` — the catalog offering plan CRN used
- `Username` — the administrator username (`admin`)
- `FGT1_Default_Admin_Password` — the initial password for FortiGate 1. The password may be empty on first boot; if so, use the instance ID as the initial password.
- `FGT2_Default_Admin_Password` — the initial password for FortiGate 2. The password may be empty on first boot; if so, use the instance ID as the initial password.

Save these values before you close the workspace. When **Terraform commands successful** and **Cart creation successful** are both displayed, your HA cross-zone firewall pair is provisioned and ready to use.

It is a good idea to review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

Before you attempt to connect, review the [planning considerations](/docs/licensed-firewall?topic=licensed-firewall-getting-started#getting-started-planning) for information about the default security group posture and license plan constraints.
{: important}

## Next steps
{: #deploy-fortigate-next-steps}

- [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall) — add a security group rule, configure routing, and log in for the first time.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — review what IBM applied during provisioning before making changes.
