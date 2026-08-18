---

copyright:
  years: 2026
lastupdated: "2026-08-18"

keywords: VNF license service, FortiGate license check, software attachment, entitlement, license service verification

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I verify that my FortiGate instance is using the VNF License Service?
{: #troubleshoot-check-vnf-license-service}
{: troubleshoot}
{: support}

Your FortiGate virtual server instance may not have a valid license entitlement from the VNF License Service.
{: shortdesc}

It is not clear whether the FortiGate instance was provisioned with a license through the VNF License Service, or whether the license entitlement is correctly associated with the instance.
{: tsSymptoms}

The VNF License Service is an internal service with no direct visibility in the IBM Cloud console, marketplace, API, or CLI. License entitlements are associated with a virtual server instance through a software attachment, which is visible through the virtual server instance details in the IBM Cloud console.
{: tsCauses}

Try the following steps to verify that the instance is using the VNF License Service:
{: tsResolve}

1. In the [IBM Cloud console](/login), navigate to **Infrastructure > Compute > Virtual server instances** and open your FortiGate instance.

1. On the instance overview page, scroll to **Image details**. Confirm that the image shown is the Fortinet vFSA image.

   ![{ALT TEXT}]({IMAGE_FILE})
   {: caption="{CAPTION}"}

1. Click the software instance link shown under the image details. Confirm that the product and pricing plan are shown.

   ![{ALT TEXT}]({IMAGE_FILE})
   {: caption="{CAPTION}"}

1. In the software instance details, confirm that a pricing plan and an entitlement ID are shown. The entitlement ID is the unique identifier that the VNF License Service stores in the FortiFlex record for this instance.

   ![{ALT TEXT}]({IMAGE_FILE})
   {: caption="{CAPTION}"}

1. If no software attachment or entitlement is shown, the instance was not provisioned through the IBM Cloud catalog automation. Delete the instance and redeploy it by using the FortiGate offering in the [IBM Cloud catalog](https://cloud.ibm.com/catalog){: external}.

1. If a software attachment is present but the FortiGate license still shows as invalid, see [Why is the FortiGate license not active after deployment?](/docs/licensed-firewall?topic=licensed-firewall-troubleshoot-licensed-firewall) for further steps.
