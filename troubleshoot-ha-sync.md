---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate HA sync, HA synchronization, HA heartbeat, FortiGate cluster, passive node

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is HA synchronization failing after deployment?
{: #troubleshoot-ha-sync}
{: troubleshoot}
{: support}

After deploying an HA Single Zone or HA Cross Zone FortiGate pair, the HA cluster fails to synchronize or the passive node does not appear in the cluster.
{: shortdesc}

The FortiGate web console or CLI shows the passive node as out of sync, missing from the cluster, or in a standalone state.
{: tsSymptoms}

HA synchronization failures are typically caused by:

- The security group is blocking traffic between the FortiGate `port3` (HA heartbeat) interfaces.
- The static IP addresses or subnets provided for `port3` are incorrect.
- The HA heartbeat subnet does not have connectivity between zones (cross-zone deployments).
{: tsCauses}

Try the following steps to resolve the issue:
{: tsResolve}

1. **Verify security group rules.**

   Confirm that the automatically created security group includes inbound rules that allow traffic from both FortiGate `port3` IP addresses. For HA Single Zone, the rules allow TCP/UDP port 703 from the HA heartbeat subnet (`SUBNET_3`). For HA Cross Zone, the rules allow all protocols from the specific `port3` IPs of each node.

2. **Check HA status from the CLI.**

   Log in to the FortiGate CLI and run the following command to verify the HA cluster state:

   ```sh
   get system ha status
   ```
   {: pre}

   Review the output for peer connectivity and synchronization status.

3. **Verify static IP assignments.**

   Confirm that the `FGT1_STATIC_IP_PORT3` and `FGT2_STATIC_IP_PORT3` values used during deployment match the actual IP addresses assigned to the `port3` interfaces on each FortiGate.

4. **Open a support case.**

   If HA synchronization is still failing after completing these steps, open a support case with IBM Support. Include the virtual server instance IDs, VPC ID, Schematics workspace ID, and the output of `get system ha status`. For more information, see [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support).
