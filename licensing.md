---

copyright:
  years: 2026
lastupdated: "2026-08-18"

keywords: FortiGate licensing, FortiFlex, license registration, FortiCare, public gateway, floating IP, license status, troubleshooting, license invalid, grace period, HA cluster

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Understanding FortiGate licensing
{: #understanding-fortigate-licensing}

Learn how FortiGate licenses are activated, what public connectivity each deployment topology requires, how to verify that a license is valid, and how to resolve common licensing issues.
{: shortdesc}

Disclaimer: Each FortiGate instance must maintain outbound connectivity to Fortinet's licensing infrastructure over the public internet for license registration and periodic license validation ("call home" requirements). The default configuration provides the required connectivity. Blocking this access can cause the license status to change to `Invalid`. See the following section for details about the connectivity required for each deployment topology.
{: important}

## Overview
{: #licensing-overview}

When you provision a FortiGate instance from the IBM Cloud catalog, IBM automatically handles the full license lifecycle. The FortiFlex license is retrieved and installed by the FortiGate image when the instance first starts. You do not need to register or apply a license manually. After provisioning, each node periodically validates its license by contacting Fortinet FortiGuard infrastructure over the public internet. Interrupting that outbound connectivity is the most common cause of license issues after deployment.

## How the FortiFlex license is installed
{: #licensing-fortiflex-install}

When the FortiGate virtual machine starts for the first time, FortiOS runs a built-in license injection process:

1. FortiOS calls the IBM Cloud instance metadata API to obtain an identity token.
1. Using that token, FortiOS calls the software attachments metadata endpoint to retrieve the FortiFlex license key associated with your virtual server instance.
1. FortiOS installs the key and restarts to complete activation.
1. After the restart, FortiOS contacts Fortinet FortiCloud to validate the license. This handshake typically completes within a few minutes but can take up to an hour.

After a successful activation, `get system status` shows `License Status: Valid`.

For the license injection to succeed, your virtual server instance must have the **metadata service** and **secure access** options enabled (visible under **Virtual server instance > Overview**), and it must have outbound internet access through a floating IP or public gateway so that it can reach FortiCloud.

### Checking FortiOS cloud-init logs
{: #licensing-cloudinit-logs}

If you suspect a problem with license injection, you can review the FortiOS cloud-init logs directly from the FortiGate CLI:

```text
diagnose debug cloudinit show
```
{: pre}

A successful run ends with a line similar to:

```text
VM license install succeeded. Will reboot firewall.
```
{: screen}

If the log shows repeated `Failed to request forticare license` lines, the most likely cause is that the instance cannot reach FortiCloud over the public internet. Verify that a floating IP or public gateway is attached and that the public security group egress rules include UDP 53, TCP 443, and TCP 8890.

### Additional Fortinet FortiFlex resources
{: #licensing-fortiflex-resources}

- [FortiFlex License Life-cycle](https://docs.fortinet.com/fortiflex){: external}
- [FortiFlex Instance Grace Period](https://docs.fortinet.com/fortiflex){: external}
- [FortiFlex Troubleshooting](https://docs.fortinet.com/fortiflex){: external}
- [FortiFlex General Documentation](https://docs.fortinet.com/fortiflex){: external}

## Public connectivity requirement
{: #licensing-public-connectivity}

Each FortiGate instance, including the secondary node in an HA deployment, must be able to reach the Fortinet licensing infrastructure over the public internet. The public security group's egress rules permit the required outbound traffic on UDP 53, TCP 443, and TCP 8890. How public connectivity is provided depends on the deployment topology.

### Single VM
{: #licensing-single-vm}

A floating IP is automatically attached to the public interface (`port1`) at deployment time. The pre-configured egress security group rules allow the instance to reach the Fortinet licensing servers without any additional configuration.

### Active/Passive HA - Single Zone
{: #licensing-ha-single-zone}

A floating IP is attached to the public interface (`port1`) and the management interface (`port4`) of the primary node. For the secondary node, the floating IP is assigned only to the management interface (`port4`).

`PUBLIC_GATEWAY_ID` is the ID of the public gateway that you attach to the `port1` subnet so that the secondary FortiGate node can reach the internet and register its license. You are prompted to supply the public gateway ID during the Schematics deployment.

### Active/Passive HA - Cross Zone
{: #licensing-ha-cross-zone}

A floating IP is attached to the public interface (`port1`) and the management interface (`port4`) of both the primary and secondary nodes. No public gateway is required.

## License registration and status
{: #licensing-registration-and-status}

The Fortinet FortiFlex infrastructure registers and installs the license during the initial startup of each FortiGate instance. After a successful registration, each vFSA node displays a `Valid` license status. You can verify the license status by running the following command on the FortiGate CLI:

```text
get system status
```
{: pre}

The output looks similar to the following example:

```text
IBM-HA-Active(Primary) # get system status
Version: FortiGate-VM64-IBM v8.0.1,...
Serial-Number: FGVMMLTM2XXX
License Status: Valid
License Expiration Date: 2027-07-29
VM Resources: 2 CPU/2 allowed, 4892 MB RAM
..........
```
{: screen}

A `Valid` status on each node confirms that the instance is fully licensed and that FortiGuard subscription services are active.

## License expiration and renewal
{: #licensing-expiration}

License expiration is handled automatically by IBM. You do not need to take any action to renew or extend the license.

## Troubleshooting
{: #licensing-troubleshooting}

### Terraform provisioning fails with a public gateway quota error
{: #licensing-troubleshooting-public-gateway}

For the Active/Passive HA - Single Zone offering, a public gateway is required on the public subnet so that the secondary node can reach FortiCloud for license registration. If you already have a public gateway in the same zone, provisioning may fail with an error similar to:

```text
Creating a new public gateway will put the user over quota.
Allocated: 1, Requested: 1, Quota: 1
```
{: screen}

To resolve this issue, choose one of the following options:

- Provide the ID of your existing public gateway in the `PUBLIC_GATEWAY_ID` Terraform input variable and attach it to the public subnet.
- Delete the existing public gateway in that zone and retry the deployment.

### License shows Invalid after provisioning
{: #licensing-troubleshooting-invalid}

If `get system status` shows `License Status: Invalid`, the FortiOS license injection process was not able to complete successfully. Check the following:

1. **Metadata service is enabled.** On the virtual server instance overview page, scroll to the bottom and confirm that the metadata service and secure access options are both enabled.
1. **Outbound internet access is available.** Each node must be able to reach FortiCloud. Confirm that a floating IP or public gateway is attached to the instance and that the public security group egress rules include UDP 53, TCP 443, and TCP 8890.
1. **Review cloud-init logs.** Run `diagnose debug cloudinit show` on the FortiGate CLI. Repeated `Failed to request forticare license` lines confirm a connectivity problem to FortiCloud.

If the instance cannot be recovered, delete the virtual server instance and redeploy. There is no in-place recovery path for a failed license injection.

### License is in grace period
{: #licensing-troubleshooting-grace-period}

If the license status shows a grace period warning, the FortiGate instance has lost periodic contact with Fortinet FortiGuard. A 30-day grace period applies before the license becomes invalid.

To diagnose the connectivity issue, run the following commands on the FortiGate CLI:

```text
diagnose debug application update -1
diagnose debug enable
execute update-now
```
{: pre}

A successful update ends with `UPDATE successful`. If the output shows repeated `SETUP failed` lines, the instance cannot reach FortiGuard.

You can also run the following command to see the error code returned by FortiGuard:

```text
diagnose hardware sysinfo vm full
```
{: pre}

For a detailed explanation of every field in the output, see [VM license — display license information from FortiGuard](https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/416169/vm-license#ipt-to-display-license-information-from-fortiguard){: external}.

Common causes and fixes:

- **Security group egress rules removed.** Restore the three default egress rules (UDP 53, TCP 443, TCP 8890) on the public interface security group.
- **Floating IP or public gateway removed.** Each node requires a route to the public internet for both license validation and IPS/antivirus signature updates. Re-attach the floating IP and, for Single Zone HA deployments, the public gateway.
- **Network ACL blocking egress traffic.** [New]{: tag-green} If a Network Access Control List (NACL) is applied to the public subnet, confirm that it includes egress rules that allow outbound traffic to FortiGuard on UDP 53, TCP 443, and TCP 8890. Modify the NACL to add the missing rules if any are blocked.

## Getting support
{: #licensing-getting-support}

If you are unable to resolve a licensing issue by using the steps in this topic, open an IBM Cloud support case. Include the following information to help IBM support route and resolve your case efficiently:

- The virtual server instance ID
- The deployment topology (Single VM, Active/Passive HA Single Zone, or Active/Passive HA Cross Zone)
- The output of `get system status` from each FortiGate node
- The output of `diagnose debug cloudinit show` if the issue is a failed license injection
- The output of `diagnose hardware sysinfo vm full` if the issue is a grace period or license validation failure
- The Schematics workspace logs if the issue occurred during provisioning

## Related links
{: #licensing-related-links}

- [Understanding the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — Review the full bootstrap configuration applied to each deployment topology at provisioning time, including interface and security group setup.
- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices) — Details on the egress security group rules that enable licensing traffic.
- [FortiFlex documentation](https://docs.fortinet.com/fortiflex){: external} — Fortinet's official FortiFlex licensing documentation, including grace period and troubleshooting guides.
