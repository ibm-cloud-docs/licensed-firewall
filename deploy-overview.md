---

copyright:
  years: 2026

lastupdated: "2026-07-15"

keywords: deploy firewall, FortiGate, single VM, HA single zone, HA cross zone, Terraform, Schematics

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a licensed FortiGate firewall
{: #fortinet-firewall-order}

You can deploy a licensed Fortinet FortiGate Next-Generation Firewall from the IBM Cloud catalog in three configurations: a single virtual machine (VM), a high-availability (HA) pair in a single zone, or an HA pair across two zones. All three offerings are deployed by using IBM Cloud Schematics, which runs the Terraform automation on your behalf. Each HA offering provisions both an active and a passive node, resulting in five distinct firewall configurations in total.
{: shortdesc}

## Before you begin
{: #deploy-fortinet-prereqs}

Before you deploy a FortiGate firewall, ensure that the following resources exist in your IBM Cloud account and that you have reviewed the [planning considerations and limitations](/docs/licensed-firewall?topic=licensed-firewall-planning).

The number of subnets required depends on the topology you are deploying:

| Topology | Subnets required |
|---|---|
| Single VM | 2 — one public (`port1`), one private (`port2`) |
| HA Single Zone | 4 — public (`port1`), private (`port2`), HA heartbeat (`port3`), HA management (`port4`) |
| HA Cross Zone | 8 — four per zone, one for each port |
{: caption="Subnet requirements by topology" caption-side="bottom"}

The security group is created automatically during deployment. You do not need to create one in advance.

HA Cross Zone deployments consume 4 floating IPs. Verify that your account has sufficient floating IP quota in the target region before deploying.
{: note}

Ensure that the following resources are available in your IBM Cloud account:

- A VPC in the target region
- Subnets in the VPC as required for your topology (see table above)
- A pre-created SSH key in the target region
- An IBM Cloud API key with sufficient permissions to create VPC resources

## Deployment topologies
{: #deploy-topologies}

Choose the deployment topic that matches your topology:

- [Deploying a FortiGate Single VM firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-single-vm)
- [Deploying a FortiGate HA Single Zone firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-ha-single-zone)
- [Deploying a FortiGate HA Cross Zone firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-ha-cross-zone)

## Next steps
{: #deploy-fortigate-next-steps}

- [Access the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall) — add a security group rule, configure routing, and log in for the first time.
- [Understand the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) — review what IBM applied during provisioning before making changes.
