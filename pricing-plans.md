---

copyright:
  years: 2026
lastupdated: "2026-06-25"

keywords: resize firewall, change license, vsi resize, firewall migration

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Changing the firewall instance profile or license plan
{: #changing-firewall-instance-profile-or-license-plan}

## Before you begin
{: #before-you-begin-updates}

When you provision a licensed firewall, the system automatically assigns a virtual server instance profile based on the license plan that you select.

The license plan and virtual server profile are linked at deployment time and cannot be modified independently after provisioning.

**Important:**

* You cannot change the instance profile or license plan of an existing firewall.
* To modify either, you must provision a new firewall instance.

## Limitations
{: #limitations}

You cannot modify the virtual server profile or license plan for an existing deployment.

The following actions are not supported:

- Resizing a virtual server profile and automatically updating the license
- Changing the license plan for an existing firewall instance
- Upgrading or downgrading the deployment size in place
- Resizing the virtual server profile does not change or upgrade the license plan

The license plan and virtual server profile are tightly coupled and are fixed after deployment.
{: important}

## Changing the instance profile or license
{: #changing-the-instance-profile-or-license}

To change the virtual server profile or license plan, you must create a new firewall deployment with the desired configuration.

1. From the IBM Cloud catalog, select the licensed firewall offering.
2. Choose the required license plan.
3. Select the deployment size that you want.
4. Provision a new firewall instance.

A new virtual server profile is automatically assigned based on your selections.

## Migrating to a new deployment
{: #migrating-to-a-new-deployment}

After provisioning the new instance, migrate your configuration to complete the change.

1. Export the configuration from the existing firewall.
2. Deploy the new instance with the required license and size.
3. Import the configuration into the new instance.
4. Validate firewall rules, routing, connectivity, and traffic flow.
5. Redirect traffic to the new instance.
6. Delete the old instance after validation is complete.
