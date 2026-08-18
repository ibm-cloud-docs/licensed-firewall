---

copyright:
  years: 2026
lastupdated: "2026-08-18"

keywords: FortiGate public gateway quota, Terraform public gateway error, HA single zone deployment, public gateway over quota

subcollection: licensed-firewall

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why did my Active/Passive HA Single Zone deployment fail with a public gateway quota error?
{: #troubleshoot-public-gateway-quota}
{: troubleshoot}
{: support}

The Schematics deployment for an Active/Passive HA Single Zone FortiGate failed with a public gateway quota error.
{: shortdesc}

The Schematics workspace log shows a Terraform error similar to the following:
{: tsSymptoms}

```text
Error: Creating a new public gateway will put the user over quota.
Allocated: 1, Requested: 1, Quota: 1
```
{: screen}

The Active/Passive HA Single Zone offering creates a public gateway on the public subnet (`port1`) so that the secondary FortiGate node can reach FortiCloud for license registration. The secondary node does not have a floating IP on `port1`, so the public gateway is its only route to the public internet. If your account already has a public gateway in the same zone, the deployment fails because creating a second public gateway would exceed the per-zone quota.
{: tsCauses}

Choose one of the following options to resolve the issue:
{: tsResolve}

1. **Use your existing public gateway.** Find the ID of the existing public gateway in your zone. In the [IBM Cloud console](/login), navigate to **Infrastructure > Network > Public gateways** and copy the ID of the gateway in the same zone as your deployment. Then go to the Schematics workspace for your FortiGate deployment, click **Settings**, and set the `PUBLIC_GATEWAY_ID` input variable to that ID. Click **Actions > Apply plan** to retry the deployment.

1. **Delete the existing public gateway and retry.** If the existing public gateway is no longer needed, delete it from **Infrastructure > Network > Public gateways** in the [IBM Cloud console](/login). Then retry the deployment by clicking **Actions > Apply plan** in the Schematics workspace.

   Deleting a public gateway that is in use removes outbound internet access for all resources in the attached subnet.
   {: important}

1. If neither option resolves the issue, [open a support case](https://cloud.ibm.com/unifiedsupport/cases/add){: external}. Include the Schematics workspace ID, the job ID of the failed apply, and the relevant log output.
