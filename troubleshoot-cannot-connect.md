---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate cannot connect, FortiGate connection timeout, security group inbound rule, floating IP, FortiGate access

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I connect to my FortiGate after deployment?
{: #troubleshoot-cannot-connect}
{: troubleshoot}
{: support}

After deploying a FortiGate firewall, you cannot connect to the FortiGate web console or SSH.
{: shortdesc}

Attempts to reach the FortiGate in a browser or over SSH time out. The floating IP is visible in your VPC resources, but the firewall does not respond.
{: tsSymptoms}

Connection attempts fail for one or more of the following reasons:

- No inbound security group rule exists to allow your administrator IP address. The security group created during deployment denies all inbound traffic by default.
- You are connecting to the wrong IP address. The public floating IP differs by topology. For HA deployments, the active node IP and the HA management IPs are separate.
- A firewall policy is blocking the traffic. The FortiGate drops traffic that does not match an allow policy, even if the security group permits it.
{: tsCauses}

Try the following steps to resolve the issue:
{: tsResolve}

1. **Add an inbound security group rule.**

   Verify that the security group attached to your FortiGate has an inbound TCP rule that allows port 443 (HTTPS) or port 22 (SSH) from your administrator IP address. For step-by-step instructions, see [Step 1: Allow management access in the security group](/docs/licensed-firewall?topic=licensed-firewall-access-firewall#access-firewall-security-group).

   Avoid using `0.0.0.0/0` as the source. Restrict access to known administrator IP addresses only.
   {: important}

1. **Confirm you are using the correct IP address.**

   Retrieve the public floating IP from your Schematics workspace output. The output variable name differs by topology.

   | Topology | Public IP output variable |
   |---|---|
   | Single VM | `FortiGate_Public_IP` |
   | HA Single Zone | `FortiGate_Public_IP` (active node `port1`) |
   | HA Cross Zone | `FGT1_Port1_Public_IP`, `FGT2_Port1_Public_IP` |
   {: caption="Public IP output variables by topology" caption-side="bottom"}

1. **Check the FortiGate firewall policy.**

   If the security group rule is in place and you are using the correct IP, verify that a firewall policy on the FortiGate allows HTTPS or SSH traffic on the management interface. Log in through an alternative access method (for example, the HA management IP on `port4`) and review the policies under **Policy & Objects > Firewall Policy**.

1. **Open a support case.**

   If the issue persists after completing these steps, open a support case with IBM Support. Include the virtual server instance ID, VPC ID, Schematics workspace ID, and a description of the steps already tried. For more information, see [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support).
