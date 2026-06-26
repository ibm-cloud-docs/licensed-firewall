---

copyright:
  years: 2026

lastupdated: "2026-06-26"

keywords: deploy firewall, FortiGate, single VM, HA single zone, HA cross zone, Terraform, Schematics

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Ordering a licensed Fortinet firewall
{: #fortinet-firewall-order}

You can deploy a licensed Fortinet FortiGate Next-Generation Firewall from the IBM Cloud catalog in three configurations: a single virtual machine (VM), a high-availability (HA) pair in a single zone, or an HA pair across two zones. All three offerings are deployed by using IBM Cloud Schematics, which runs the Terraform automation on your behalf.
{: shortdesc}

## Before you begin
{: #deploy-fortinet-prereqs}

Before you deploy a FortiGate firewall, ensure that the following resources exist in your IBM Cloud account:

- A VPC in the target region
- At least two subnets in the VPC: one public subnet for port1 and one private subnet for port2
- A pre-created SSH key in the target region
- A security group to attach to the FortiGate network interfaces
- An IBM Cloud API key with sufficient permissions to create VPC resources

## Deploying a FortiGate Single VM firewall
{: #deploy-fortigate-single-vm}

The Single VM offering deploys a single FortiGate virtual machine into your VPC.

1. Log in to the [IBM Cloud console](https://cloud.ibm.com){: external}.
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate Next-Generation Firewall** and select the **Single VM - PAYG - TF** tile.
1. Select the **product version** from the version dropdown.
1. Under **Deploy your workspace**, confirm that **IBM Cloud Schematics** is selected as the deployment method.
1. In the **Configure your workspace** section, review or update the workspace name, location, and resource group.
1. Scroll down to the **Input variables** section and complete the following required fields:

   | Variable | Description |
   |---|---|
   | `catalog_offering_plan_crn` | Click the field to open the plan selector. Select the plan that matches your required license tier (ATP, UTP, or Enterprise) and vCPU size. The cost per CPU hour is displayed for each option. |
   | `cluster_name` | A name for your FortiGate deployment. A random suffix is appended automatically to prevent naming collisions. |
   | `ibmcloud_api_key` | Your IBM Cloud API key. |
   | `profile` | The virtual server instance profile that determines the vCPU and memory allocation (for example, `cx2-2x4`). IBM assigns this based on the plan you select. |
   | `region` | The IBM Cloud region where the firewall is deployed (for example, `us-south`). |
   | `security_group` | The ID of the security group to attach to the FortiGate network interfaces. |
   | `ssh_public_key` | The name of a pre-created SSH key in the target region. |
   | `subnet1` | The ID of the primary public subnet used for port1 on the FortiGate. |
   | `subnet2` | The ID of the secondary private subnet used for port2 on the FortiGate. |
   | `user_data` | The custom bootstrap data file name (default: `user_data.conf`). |
   | `vpc` | The name of the VPC where the FortiGate is deployed. |
   | `zone1` | The deployment zone within the region (for example, `us-south-1`). |
   {: caption="Single VM input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the following output values:

- `FortiGate_Public_IP` — the public IP address of the FortiGate instance
- `Username` — the administrator username (`admin`)
- `Default_Admin_Password` — the initial administrator password

Save these values before you close the workspace — you need them to log in to the FortiGate web console for the first time. When **Terraform commands successful** and **Cart creation successful** are both displayed, your firewall is provisioned and ready to use.

Always review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

The license plan that you select at deployment is permanent and cannot be changed after provisioning. You are billed for that license on the virtual server instance for as long as it runs. If you need a different license plan, you must place a new order and cancel the existing deployment. However, you can resize the virtual server instance without changing the license. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
{: important}

## Deploying a FortiGate HA Single Zone firewall
{: #deploy-fortigate-ha-single-zone}

The HA Single Zone offering deploys an active-passive FortiGate HA pair within a single availability zone.

1. Log in to the [IBM Cloud console](https://cloud.ibm.com){: external}.
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate Next-Generation Firewall** and select the **HA Single Zone - PAYG - TF** tile.
1. Select the **product version** from the version dropdown.
1. Under **Deploy your workspace**, confirm that **IBM Cloud Schematics** is selected as the deployment method.
1. In the **Configure your workspace** section, review or update the workspace name, location, and resource group.
1. Scroll down to the **Input variables** section and complete the following required fields:

   | Variable | Description |
   |---|---|
   | `catalog_offering_plan_crn` | Click the field to open the plan selector. Select the plan that matches your required license tier and vCPU size. The cost per CPU hour is displayed for each option. |
   | `cluster_name` | A name for your FortiGate HA deployment. |
   | `ibmcloud_api_key` | Your IBM Cloud API key. |
   | `region` | The IBM Cloud region where the firewall is deployed. |
   | `security_group` | The ID of the security group to attach to the FortiGate network interfaces. |
   | `ssh_public_key` | The name of a pre-created SSH key in the target region. |
   | `subnet1` | The ID of the primary public subnet used for port1. |
   | `subnet2` | The ID of the secondary private subnet used for port2. |
   | `user_data` | The custom bootstrap data file name (default: `user_data.conf`). |
   | `vpc` | The name of the VPC where the FortiGate is deployed. |
   | `zone1` | The deployment zone within the region. |
   {: caption="HA Single Zone input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the output values including the public IP address, administrator username, and initial administrator password. Save these values before you close the workspace — you need them to log in to the FortiGate web console for the first time. When **Terraform commands successful** and **Cart creation successful** are both displayed, your HA firewall pair is provisioned and ready to use.

Always review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

The license plan that you select at deployment is permanent and cannot be changed after provisioning. You are billed for that license on the virtual server instance for as long as it runs. If you need a different license plan, you must place a new order and cancel the existing deployment. However, you can resize the virtual server instance without changing the license. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
{: important}

## Deploying a FortiGate HA Cross Zone firewall
{: #deploy-fortigate-ha-cross-zone}

The HA Cross Zone offering deploys an active-passive FortiGate HA pair across two availability zones for higher resiliency.

1. Log in to the [IBM Cloud console](https://cloud.ibm.com){: external}.
1. Click **Catalog** in the navigation bar.
1. Search for **Fortinet FortiGate Next-Generation Firewall** and select the **HA Cross Zone - PAYG - TF** tile.
1. Select the **product version** from the version dropdown.
1. Under **Deploy your workspace**, confirm that **IBM Cloud Schematics** is selected as the deployment method.
1. In the **Configure your workspace** section, review or update the workspace name, location, and resource group.
1. Scroll down to the **Input variables** section and complete the following required fields:

   | Variable | Description |
   |---|---|
   | `catalog_offering_plan_crn` | Click the field to open the plan selector. Select the plan that matches your required license tier and vCPU size. The cost per CPU hour is displayed for each option. |
   | `cluster_name` | A name for your FortiGate HA deployment. |
   | `ibmcloud_api_key` | Your IBM Cloud API key. |
   | `region` | The IBM Cloud region where the firewall is deployed. |
   | `security_group` | The ID of the security group to attach to the FortiGate network interfaces. |
   | `ssh_public_key` | The name of a pre-created SSH key in the target region. |
   | `subnet1` | The ID of the primary public subnet in zone 1 used for port1. |
   | `subnet2` | The ID of the secondary private subnet in zone 1 used for port2. |
   | `user_data` | The custom bootstrap data file name (default: `user_data.conf`). |
   | `vpc` | The name of the VPC where the FortiGate is deployed. |
   | `zone1` | The first deployment zone within the region (for example, `us-south-1`). |
   | `zone2` | The second deployment zone within the region (for example, `us-south-2`). |
   {: caption="HA Cross Zone input variables" caption-side="bottom"}

1. Click **Install**.

IBM Cloud Schematics creates a workspace and runs the Terraform automation. You can watch the Terraform execution in the **Log** section of the workspace. When the deployment completes successfully, the log displays the output values including the public IP address, administrator username, and initial administrator password. Save these values before you close the workspace — you need them to log in to the FortiGate web console for the first time. When **Terraform commands successful** and **Cart creation successful** are both displayed, your HA cross-zone firewall pair is provisioned and ready to use.

Always review the full log output for errors or warnings, even when the deployment reports as successful.
{: note}

The license plan that you select at deployment is permanent and cannot be changed after provisioning. You are billed for that license on the virtual server instance for as long as it runs. If you need a different license plan, you must place a new order and cancel the existing deployment. However, you can resize the virtual server instance without changing the license. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
{: important}
