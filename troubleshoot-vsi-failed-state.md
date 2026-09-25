---

copyright:
  years: 2026
lastupdated: "2026-09-25"

keywords: FortiGate VSI failed, virtual server instance failed, provisioning failed state, FortiGate deployment failed

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my FortiGate virtual server instance in a Failed state?
{: #troubleshoot-vsi-failed-state}
{: troubleshoot}
{: support}

After ordering a FortiGate instance from the IBM Cloud catalog, the virtual server instance is in a `Failed` state.
{: shortdesc}

The virtual server instance shows a `Failed` status in the IBM Cloud console. The instance is shut down and the FortiGate is not accessible.
{: tsSymptoms}

When a FortiGate instance is provisioned, several IBM Cloud components are involved in sequence: Catalog Manager, Resource Controller, License Manager, and the VNF License Provider. If an error occurs in any of these components during provisioning, the virtual server instance is marked as `Failed` and shut down automatically. Common causes include:
{: tsCauses}

- An error occurred in the VNF License Provider while retrieving or creating the FortiFlex license token.
- The Fortinet FortiFlex infrastructure was unavailable at provisioning time.
- An error occurred in an upstream IBM Cloud component such as Catalog Manager or License Manager.

Try the following steps to resolve the issue:
{: tsResolve}

1. Delete the failed virtual server instance. There is no in-place recovery path for a virtual server instance in a `Failed` state.

1. If a FortiFlex outage is known or suspected, wait for the outage to be resolved before retrying. See [What do I do if the Fortinet FortiFlex infrastructure is not accessible?](/docs/licensed-firewall?topic=licensed-firewall-troubleshoot-fortiflex-not-accessible) for steps to check FortiFlex availability.

1. Retry the deployment by placing a new order from the [IBM Cloud catalog](https://cloud.ibm.com/catalog){: external} with the same input values.

1. If the virtual server instance fails again, [open an IBM support case](https://cloud.ibm.com/unifiedsupport/cases/add){: external}. Include the virtual server instance ID, the Schematics workspace ID if applicable, and a description of the failure.
