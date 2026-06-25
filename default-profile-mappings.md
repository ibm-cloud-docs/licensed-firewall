---

copyright:
  years: 2026
lastupdated: "2026-06-25"

keywords: virtual server mapping, firewall profiles, instance profiles, license mapping

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Default virtual server profile mappings for licensed firewalls
{: #default-vsi-profile-mappings}

Each license plan entitles you to a specific number of vCPUs for your FortiGate virtual firewall (vFSA). IBM assigns the virtual server instance profile that allocates those vCPUs based on the plan and deployment size you select. The assigned profile provides enough throughput to support the pricing plan that you select. IBM also manages profile selection because not all profiles are available in all regions. You cannot choose or override the profile. For example, an Enterprise plan with 8 vFSA vCPUs is allocated the `cx3d-8x20` virtual server instance profile.
{: shortdesc}

For guidance on choosing a license plan and deployment size, see [Understanding firewall license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles).

## Enterprise
{: #enterprise}

This license plan uses gen3-cx profiles and supports Medium, Large, and X-large deployment sizes.

| Deployment size | vCPU | Instance profile | Profile family |
|----------------|------|------------------|----------------|
| Medium         | 8    | cx3d-8x20        | gen3-cx        |
| Large          | 16   | cx3d-16x40       | gen3-cx        |
| X-large        | 32   | cx3d-32x80       | gen3-cx        |
{: caption="Enterprise license plan virtual server profile mappings" caption-side="bottom"}

## Unified Threat Protection (UTP)
{: #unified-threat-protection-utp}

This license plan uses gen3-cx profiles and supports Small, Medium, and Large deployment sizes.

| Deployment size | vCPU | Instance profile | Profile family |
|----------------|------|------------------|----------------|
| Small          | 2    | cx3d-2x5         | gen3-cx        |
| Medium         | 8    | cx3d-8x20        | gen3-cx        |
| Large          | 16   | cx3d-16x40       | gen3-cx        |
{: caption="Unified Threat Protection (UTP) license plan virtual server profile mappings" caption-side="bottom"}

## Advanced Threat Protection (ATP)
{: #advanced-threat-protection-atp}

This license plan uses gen2-cx profiles and supports Small and Medium deployment sizes. Note that gen2-cx profiles are not available in all regions. The `cx2-2x4` and `cx2-8x16` profiles are not available in Mumbai, Chennai, and Montreal.

| Deployment size | vCPU | Instance profile | Profile family |
|----------------|------|------------------|----------------|
| Small          | 2    | cx2-2x4          | gen2-cx        |
| Medium         | 8    | cx2-8x16         | gen2-cx        |
{: caption="Advanced Threat Protection (ATP) license plan virtual server profile mappings" caption-side="bottom"}

**Notes:**

- Virtual server profiles are automatically assigned during provisioning based on the selected license plan and deployment size.
- You cannot manually select or override the instance profile during deployment.
- Available deployment sizes vary by license plan.
