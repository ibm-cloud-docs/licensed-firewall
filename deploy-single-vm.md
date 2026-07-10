---

copyright:
  years: 2026

lastupdated: "2026-07-15"

keywords: deploy firewall, FortiGate, single VM, Terraform, Schematics

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a FortiGate Single VM firewall
{: #deploy-single-vm}

The Single VM offering deploys one FortiGate next-generation firewall virtual machine into your VPC. It provides a single public interface (`port1`) and a single private interface (`port2`) with no redundancy. This topology is suited for development, testing, or workloads where a brief interruption during instance recovery is acceptable.
{: shortdesc}

Before you begin, complete the prerequisites in [Deploying a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-fortinet-firewall-order#deploy-fortinet-prereqs).

Terraform deploys the following resources:

- One FortiGate licensed instance with two network interfaces (`port1` and `port2`)
- One floating public IP address attached to `port1`
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
   | `SUBNET1` | The ID of the primary, public subnet used for `port1` on the FortiGate. |
   | `SUBNET2` | The ID of the secondary, private subnet used for `port2` on the FortiGate. |
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

Before you attempt to connect, review the [planning considerations and limitations](/docs/licensed-firewall?topic=licensed-firewall-planning) for information about the default security group posture and license plan constraints.
{: important}
