---

copyright:
  years: 2026
lastupdated: "2026-08-18"

keywords: FortiGate license grace period, FortiFlex grace period, FortiGuard connectivity, license expiring, license warning

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my FortiGate license in a grace period?
{: #troubleshoot-license-grace-period}
{: troubleshoot}
{: support}

The FortiGate license shows a grace period warning instead of a valid status.
{: shortdesc}

Running `get system status` on the FortiGate CLI shows that the license is in a grace period. FortiGuard subscription services may be degraded or unavailable.
{: tsSymptoms}

The FortiFlex license requires periodic contact with Fortinet's FortiGuard servers over the public internet to remain valid. If the FortiGate instance loses that outbound connectivity, a 30-day grace period begins. If connectivity is not restored within 30 days, the license becomes invalid. Common causes include:
{: tsCauses}

- The egress rules on the public interface security group were removed or modified, blocking outbound traffic to FortiGuard.
- The floating IP was detached from `port1`, removing the instance's route to the public internet.
- For Active/Passive HA Single Zone deployments, the public gateway was removed from the public subnet, leaving the secondary node without internet access.

Try the following steps to restore connectivity and resolve the grace period:
{: tsResolve}

1. Check whether the FortiGate can reach FortiGuard. Run the following commands on the FortiGate CLI:

   ```sh
   diagnose debug application update -1
   diagnose debug enable
   execute update-now
   ```
   {: pre}

   A successful result ends with `UPDATE successful`. If the output shows repeated `SETUP failed` lines, the FortiGate cannot reach FortiGuard.

1. Identify the error code. Run the following command to see the response code from FortiGuard:

   ```sh
   diagnose hardware sysinfo vm full
   ```
   {: pre}

   For a detailed explanation of every field in the output, see [VM license — display license information from FortiGuard](https://docs.fortinet.com/document/fortigate/8.0.0/administration-guide/416169/vm-license#ipt-to-display-license-information-from-fortiguard){: external}.

   A `code` value of `502` or similar indicates that FortiGuard is returning an error, which typically means the request is reaching Fortinet but is being rejected — usually because outbound traffic is being blocked upstream.

1. Check the security group egress rules. On the IBM Cloud console, open the security group attached to the public interface (`port1`) and confirm that it includes the following egress rules. If any are missing, add them back.

   | Protocol | Port | Destination |
   |---|---|---|
   | UDP | 53 | `0.0.0.0/0` |
   | TCP | 443 | `0.0.0.0/0` |
   | TCP | 8890 | `0.0.0.0/0` |
   {: caption="Required egress rules for FortiGuard connectivity" caption-side="bottom"}

   The following image shows a security group configuration with the required egress rules in place:

   ![{ALT TEXT}]({IMAGE_FILE})
   {: caption="{CAPTION}"}

   The following image shows a security group configuration that is missing the required egress rules:

   ![{ALT TEXT}]({IMAGE_FILE})
   {: caption="{CAPTION}"}

1. Verify that the floating IP and public gateway are attached. Each FortiGate node must have a route to the public internet. Confirm that a floating IP is attached to `port1` on each node. For Active/Passive HA Single Zone deployments, also confirm that a public gateway is attached to the public subnet for the secondary node.

1. After restoring connectivity, re-run `execute update-now` to confirm that the update succeeds. The grace period clears automatically once FortiGuard successfully validates the license.

1. If the grace period warning persists after restoring connectivity, [open a support case](https://cloud.ibm.com/unifiedsupport/cases/add){: external}. Include the virtual server instance ID, the output of `diagnose hardware sysinfo vm full`, and the output of `execute update-now`.
