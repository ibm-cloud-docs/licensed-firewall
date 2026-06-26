---

copyright:
  years: 2026
lastupdated: "2026-06-26"

keywords: firewall, license plans, vsi profiles, instance sizing, deployment sizes

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

```md
# About firewall license plans and instance profiles
{: #about-firewall-license-plans-and-instance-profiles}

When you provision a licensed firewall, the system automatically assigns a virtual server instance profile based on the license plan that you select.
{: shortdesc}

The license plan determines:

- The instance profile family
- The supported deployment sizes
- The performance and scaling characteristics

The license plan and virtual server profile are linked at deployment time.

## Choosing a license plan
{: #choosing-a-license-plan}

Select a license plan based on your workload requirements, performance needs, and scale.

| License plan | Best for | Supported sizes | Profile family | Key characteristics |
|--------------|----------|------------------|----------------|--------------------|
| ATP (Advanced Threat Protection) | Entry-level deployments | Small, Medium | gen2-cx | Lower cost, limited scale |
| UTP (Unified Threat Protection) | General-purpose security | Small, Medium, Large | gen3-cx | Balanced cost and performance |
| Enterprise | High-performance environments | Medium, Large, X-large | gen3-cx | Highest scalability and throughput |
{: caption="License plan comparison" caption-side="bottom"}

## Sizing and scaling characteristics
{: #sizing-and-scaling-characteristics}

Understanding the sizing and scaling characteristics helps you select the appropriate deployment size for your workload requirements.

### Deployment sizes
{: #deployment-size-definitions}

Deployment sizes are mapped to vCPU allocations.

| Deployment size | vCPU |
|----------------|------|
| Small | 2 |
| Medium | 8 |
| Large | 16 |
| X-large | 32 |
{: caption="Deployment size vCPU allocations" caption-side="bottom"}

- Larger deployment sizes include increased memory, which improves session handling and overall scalability.

### Supported sizes by license plan
{: #supported-deployment-sizes-by-license-plan}

The available deployment sizes vary by license plan.

| License plan | Small (2 vCPU) | Medium (8 vCPU) | Large (16 vCPU) | X-large (32 vCPU) |
|--------------|----------------|------------------|------------------|--------------------|
| Enterprise | Not available | Supported | Supported | Supported |
| UTP | Supported | Supported | Supported | Not available |
| ATP | Supported | Supported | Not available | Not available |
{: caption="Supported deployment sizes by license plan" caption-side="bottom"}

### Performance
{: #performance-considerations}

Firewall performance scales with deployment size and enabled security features, with larger deployments providing higher throughput.

| Deployment size | Typical NGFW throughput | Typical IPS throughput |
|----------------|------------------------|------------------------|
| Small (2 vCPU) | ~1–2 Gbps | ~2 Gbps |
| Medium (8 vCPU) | ~4–5 Gbps | ~6 Gbps |
| Large (16 vCPU) | ~9–10 Gbps | ~11–12 Gbps |
| X-large (32 vCPU) | ~14–16 Gbps | ~16–22 Gbps |
{: caption="Performance characteristics by deployment size" caption-side="bottom"}

**Notes:**

- Values vary based on configuration and traffic profile.
- Enabling advanced security services (for example, IPS or threat protection) can reduce throughput.

### Session scaling
{: #session-scaling}

Connection capacity increases with deployment size and available memory.

- Smaller deployments support fewer concurrent sessions.
- Larger deployments support significantly higher connection volumes and session tables.

### VDOM support
{: #vdom-support}

The number of supported virtual domains (VDOMs) varies by deployment size.

| Deployment size | VDOM support |
|----------------|--------------|
| Small | Limited |
| Medium | Moderate |
| Large | High |
| X-large | Maximum |
{: caption="VDOM support by deployment size" caption-side="bottom"}

- VDOMs enable segmentation and multi-tenant configurations.
- Higher deployment sizes support more complex environments and greater isolation.

## Instance profile details
{: #instance-profile-details}

Instance profiles define the compute resources allocated to your firewall deployment.

Each license plan includes a specific number of vCPUs for your FortiGate virtual firewall (vFSA). Based on the selected license plan and deployment size, IBM automatically assigns a virtual server instance profile that provides the required vCPUs and throughput. Because available profiles vary by region, IBM manages profile selection and you cannot choose or override the assigned profile. For example, an Enterprise plan with 8 vFSA vCPUs is deployed with the `cx3d-8x20` virtual server instance profile.

### Profile families
{: #instance-profile-families}

Each license plan uses a specific instance profile family.

| License plan | Profile family |
|--------------|----------------|
| Enterprise | gen3-cx |
| UTP | gen3-cx |
| ATP | gen2-cx |
{: caption="Instance profile families by license plan" caption-side="bottom"}

- Gen3 profiles use newer infrastructure and are recommended for most deployments.
- Gen2 profiles are typically used for smaller or entry-level workloads.

### Profile considerations
{: #instance-profile-considerations}

Consider the following factors when evaluating instance profiles for your deployment:

- `cx` profiles provide a balanced cost-to-performance ratio and are suitable for most workloads.
- Profiles with higher memory ratios can benefit environments with high session counts.
- Profiles with `-d` include additional instance storage, which can increase cost.
- Profile selection impacts both performance characteristics and pricing.

## Planning considerations
{: #considerations}

Review the following considerations before selecting your license plan and deployment size:

- Larger deployment sizes provide higher throughput and session capacity.
- The selected license plan determines the available sizing options and scaling limits.
- Some instance configurations might be less suitable for high availability or hub-and-spoke architectures. Review network design requirements before selecting a deployment.
- All deployments include FortiCare Premium support.
- Availability varies by region. Check the IBM Cloud catalog for supported locations.

The license plan and virtual server profile are linked and cannot be changed after deployment.
{: important}
```
