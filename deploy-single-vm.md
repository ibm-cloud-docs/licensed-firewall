---

copyright:
  years: 2026
lastupdated: "2026-08-10"

keywords: deploy firewall, FortiGate, single VM, Terraform, Schematics

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a FortiGate Single VM firewall
{: #deploy-single-vm}

The Single VM offering deploys one FortiGate next-generation firewall virtual machine into your VPC. It provides a single public interface (`port1`) and a single private interface (`port2`) with no redundancy. This topology is suited for development, testing, or workloads where a brief interruption during instance recovery is acceptable.
{: shortdesc}

## Before you begin
{: #deploy-single-vm-prereqs}

Before you deploy, ensure that you have reviewed [Planning for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-planning) and that the following resources are available in your IBM Cloud account:

- A VPC in the target region
- 2 subnets in the VPC — one public (`port1`), one private (`port2`)
- A pre-created SSH key in the target region
- An IBM Cloud API key with sufficient permissions to create VPC resources

Two security groups are created automatically during deployment — one for the public interface and one for the private interface. You do not need to create them in advance.

## Deploying a Single VM firewall
{: #deploy-single-vm-steps}

Terraform deploys the following resources:

- One FortiGate licensed instance with two network interfaces (`port1` and `port2`)
- One floating public IP address attached to `port1`
- One log disk
- Two security groups — one for the public interface (`port1`) and one for the private interface (`port2`) — each with restrictive inbound rules that allow the instance to download the license and enable cluster synchronization, and allow-all outbound rules
- A bootstrap configuration

Follow these steps:

1. Log in to the [IBM Cloud console](/login).
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate VM NGFW** and select the **Fortinet FortiGate VM NGFW - Single** tile.
1. In **Configure your workspace**, review or update the following fields:
   - **Name** — A name for the Schematics workspace. A default name is pre-filled.
   - **Location** — The region where the Schematics workspace is created.
   - **Resource group** — The resource group for the workspace.
   - **Tags** — Optional tags to apply to the workspace.
1. Scroll down to **Set the input variables** and complete the fields in the **Required input variables** table:

   | Parameter | Description |
   |---|---|
   | `CATALOG_OFFERING_PLAN_CRN` | Click the field to open the plan selector. Select the plan that matches your required license tier (ATP, UTP, or Enterprise) and vCPU size. The cost per CPU hour is displayed for each option. |
   | `CLUSTER_NAME` | A name for your FortiGate deployment. A random suffix is appended automatically to prevent naming collisions. |
   | `IBMCLOUD_API_KEY` | Your IBM Cloud API key. |
   | `REGION` | The IBM Cloud region where the firewall is deployed (for example, `us-south`). |
   | `SSH_PUBLIC_KEY` | The name of a pre-created SSH key in the target region. |
   | `SUBNET1` | The ID of the primary, public subnet used for `port1` on the FortiGate. |
   | `SUBNET2` | The ID of the secondary, private subnet used for `port2` on the FortiGate. |
   | `VPC` | The name of the VPC where the FortiGate is deployed. |
   | `ZONE_1` | The deployment zone within the region (for example, `us-south-1`). |
   | `RESOURCE_GROUP` | The resource group name to attach to the FortiGate instance. |
   {: caption="Single VM required input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FortiGate_Public_IP` — Public IP address of the FortiGate instance
- `Security_Group_ID` — ID of the automatically created security group
- `Security_Group_Name` — Name of the security group
- `selected_vsi_profile` — Virtual server instance profile automatically selected based on the plan CRN
- `CATALOG_OFFERING_VERSION_CRN` — Catalog offering version CRN used
- `CATALOG_OFFERING_PLAN_CRN` — Catalog offering plan CRN used
- `Username` — Administrator username (`admin`)
- `Default_Admin_Password` — Initial administrator password. May be empty on first boot; if so, use the instance ID as the initial password.

Save these values before you close the workspace. You need them to log in to the FortiGate web console for the first time. When **Terraform commands successful** and **Cart creation successful** are both displayed, your firewall is provisioned and ready to use.

It is a good idea to review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

## Next steps
{: #deploy-single-vm-next-steps}

- [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall) — Add a security group rule, configure routing, and log in for the first time.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — Review what IBM applied during provisioning before making changes.
