---

copyright:
  years: 2026
lastupdated: "2026-09-28"

keywords: FortiFlex not accessible, Fortinet license infrastructure, FortiGate license failure, FortiFlex outage, license service unavailable

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# What do I do if the Fortinet FortiFlex infrastructure is not accessible?
{: #troubleshoot-fortiflex-not-accessible}
{: troubleshoot}
{: support}

The FortiGate instance cannot reach the Fortinet FortiFlex licensing infrastructure, and license registration or validation is failing.
{: shortdesc}

The FortiGate license status shows as `Invalid` and the instance cannot register or validate its license. The failure occurs even though outbound internet connectivity from the instance appears to be working correctly.
{: tsSymptoms}

The VNF License Service depends on the Fortinet FortiFlex API to create, start, stop, and retrieve licenses. If the FortiFlex API is experiencing an outage or degraded availability, license operations will fail.
{: tsCauses}

Try the following steps to determine whether FortiFlex is the cause and to work around the issue:
{: tsResolve}

1. Check the Fortinet FortiFlex service status. Visit the [FortiFlex API Dashboard](https://status.fortimonitor.forticloud.com/Fortiflex_API){: external} to check for any reported outages or degraded service affecting FortiFlex.

1. Check outbound connectivity from the FortiGate instance. Confirm that the public security group egress rules include UDP 53, TCP 443, and TCP 8890, and that a floating IP or public gateway is attached. From the FortiGate CLI, run:

   ```sh
   execute ping guard.fortinet.net
   ```
   {: pre}

   If the ping fails, the issue is local connectivity rather than a FortiFlex outage. For more information, see [Why is the FortiGate license not active after deployment?](/docs/licensed-firewall?topic=licensed-firewall-troubleshoot-licensed-firewall).

1. If a FortiFlex outage is confirmed, wait for the outage to be resolved before retrying license registration. There is no in-place recovery while FortiFlex is unavailable.

1. If the instance failed to provision because FortiFlex was unavailable at provisioning time, delete the virtual server instance and retry the deployment after the outage is resolved.

1. If the issue persists after FortiFlex availability is confirmed, [open an IBM support case](/unifiedsupport/cases/add){: external}. Include the virtual server instance ID, the output of `get system status`, and the output of `diagnose debug cloudinit show`.
