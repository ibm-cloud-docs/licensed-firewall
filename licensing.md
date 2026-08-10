---

copyright:
  years: 2026
lastupdated: "2026-08-10"

keywords: FortiGate licensing, FortiFlex, license registration, FortiCare, public gateway, floating IP, license status

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Understanding FortiGate licensing
{: #understanding-fortigate-licensing}

Learn how FortiGate licenses are activated, what public connectivity each deployment topology requires, and how to verify that a license is valid.
{: shortdesc}

## Public connectivity requirement
{: #licensing-public-connectivity}

Each FortiGate instance, including the secondary node in an HA deployment, must be able to reach the Fortinet licensing infrastructure over the public internet. The public security group's egress rules permit the required outbound traffic on UDP 53, TCP 443, and TCP 8890. How public connectivity is provided depends on the deployment topology.

### Single VM
{: #licensing-single-vm}

A floating IP is automatically attached to the public interface (`port1`) at deployment time. The pre-configured egress security group rules allow the instance to reach the Fortinet licensing servers without any additional configuration.

### Active/Passive HA - Single Zone
{: #licensing-ha-single-zone}

A floating IP is attached to the public interface (`port1`) of the primary node only. The secondary node does not have a floating IP, so a public gateway must be attached to the subnet on `port1` to provide outbound internet access for licensing. You are prompted to supply the public gateway ID during the Schematics deployment.

### Active/Passive HA - Cross Zone
{: #licensing-ha-cross-zone}

A floating IP is attached to the public interface (`port1`) of both the primary and secondary nodes. No public gateway is required.

## License registration and status
{: #licensing-registration-and-status}

The Fortinet FortiFlex infrastructure registers and installs the license during the initial startup of each FortiGate instance. After a successful registration, each node displays a `Valid` license status. You can verify the license status by running the following command on the FortiGate CLI:

```
get system status
```
{: pre}

The output looks similar to the following example:

```
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

## Related links
{: #licensing-related-links}

- [Understanding the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — Review the full bootstrap configuration applied to each deployment topology at provisioning time, including interface and security group setup.
- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices) — Details on the egress security group rules that enable licensing traffic.
