---

copyright:
  years: 2026

lastupdated: "2026-07-15"

keywords: resize firewall, change license plan, vsi resize, firewall profile, firewall migration

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Resizing a firewall virtual server instance
{: #changing-firewall-instance-profile-or-license-plan}

After you deploy a licensed firewall, you can resize the virtual server instance to change the number of vCPUs and memory. However, resizing the virtual server instance does not change the license plan. The license plan is fixed at deployment time and you continue to be billed for it regardless of any resize operations.
{: shortdesc}

Resizing the virtual server instance does not change, upgrade, or cancel the license that you are paying for. The only way to change your license is to place a new order with the required license plan and cancel the existing deployment.
{: important}

## Resizing the virtual server instance
{: #resize-vsi}

You must stop the virtual server instance before you can resize it.
{: note}

1. In the [IBM Cloud console](/login), click the navigation menu and select **Infrastructure > Compute > Virtual server instances**.
1. Click the virtual server instance that you deployed to open its Details page.
1. From the Details page, resize the instance by following the steps in [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=ui).

The virtual server instance restarts automatically after the resize is complete. The license plan and associated billing remain unchanged.

## Changing the license plan
{: #changing-the-license-plan}

The license plan cannot be changed on an existing firewall deployment. To use a different license plan, you must place a new order and cancel the existing one.

1. From the [IBM Cloud catalog](https://cloud.ibm.com/catalog){: external}, deploy a new firewall instance with the required license plan and deployment size.
1. [Export the configuration from the existing firewall](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config#backup-web-console).
1. [Import the configuration into the new instance](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config#restore-web-console).
1. Validate firewall rules, routing, connectivity, and traffic flow.
1. Redirect traffic to the new instance.
1. Cancel the existing firewall deployment.

You are billed for both deployments until the existing one is cancelled.
{: note}

## Related links
{: #pricing-plans-related-links}

- [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall)
- [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config)
