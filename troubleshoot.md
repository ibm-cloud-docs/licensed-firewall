---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate license not active, license error, fortigate license, metadata service, cloud-init license

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is the FortiGate license not active after deployment?
{: #troubleshoot-licensed-firewall}
{: troubleshoot}
{: support}

After deploying a FortiGate instance, the license status shows as inactive or unlicensed in the FortiGate web console.
{: shortdesc}

When you log in to the FortiGate web console and navigate to **System > FortiGuard**, the license status is shown as **Invalid**, **Expired**, or **Not registered**, and FortiGuard services are not available.
{: tsSymptoms}

The FortiGate license is retrieved during initial startup by cloud-init through the Instance Metadata Service (IMDS). License activation can fail for several reasons:

- The Instance Metadata Service was not enabled at deployment time.
- Cloud-init did not complete successfully, preventing the license retrieval script from running.
- The instance could not reach the Fortinet FortiFlex platform during startup, for example due to a missing public gateway or routing issue.
- The FortiGate instance was deployed without using the IBM Cloud Schematics automation, which handles license provisioning.
{: tsCauses}

Try the following steps to resolve the issue:
{: tsResolve}

1. **Verify that the Instance Metadata Service is enabled.**

   In the [IBM Cloud console](/login), navigate to **VPC Infrastructure > Compute > Virtual server instances** and open your FortiGate instance. Under **Instance details**, confirm that **Metadata service** is set to **Enabled**. If it is disabled, enable it and restart the instance.

2. **Review the Schematics workspace log.**

   Open the Schematics workspace that was used to deploy the firewall and review the Terraform log for errors. Confirm that both **Terraform commands successful** and **Cart creation successful** are displayed at the end of the log.

3. **Check outbound connectivity from the FortiGate.**

   The FortiGate must be able to reach Fortinet's licensing servers on the internet. Verify that the VPC has a public gateway attached to the subnet used by port1, or that a floating IP is assigned to port1. From the FortiGate CLI, run:

   ```sh
   execute ping guard.fortinet.net
   ```
   {: pre}

   If the ping fails, check VPC routing and security group outbound rules.

4. **Restart the FortiGate instance.**

   If the Instance Metadata Service is now enabled and outbound connectivity is confirmed, stop and start the virtual server instance to trigger cloud-init to run again.

5. **Open a support case.**

   If the license is still not active after completing these steps, open a support case with IBM Support. Include the virtual server instance ID, VPC ID, Schematics workspace ID, and the relevant Schematics log output. For more information, see [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support).
