---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: firewall, license plans, vsi profiles, instance sizing, deployment sizes

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# About firewall license plans and instance profiles
{: #about-firewall-license-plans-and-instance-profiles}

When you provision a licensed firewall, the system automatically assigns a virtual server instance profile based on the license plan that you select.
{: shortdesc}

The license plan determines:

- The instance profile family
- The supported deployment sizes
- The performance and scaling characteristics

The license plan and virtual server profile are linked at deployment time.

## Planning considerations
{: #considerations}

Review the following considerations before selecting your license plan and deployment size:

- Larger deployment sizes provide higher throughput and session capacity.
- The selected license plan determines the available sizing options and scaling limits.
- Some instance configurations might be less suitable for high availability or hub-and-spoke architectures. Review network design requirements before selecting a deployment.
- All deployments include FortiCare Premium support.
- Availability varies by region. Check the [IBM Cloud catalog](https://cloud.ibm.com/catalog){: external} for supported locations.

The license plan cannot be changed after deployment. You can resize the virtual server instance to a different profile without changing the license plan. For more information, see [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan).
{: important}

## Choosing a deployment topology
{: #choosing-a-deployment-topology}

Select a deployment topology based on your availability requirements and tolerance for downtime. All three topologies are available as separate catalog tiles and are deployed by using IBM Cloud Schematics.

| Topology | Catalog Tile | Firewall Instances | Availability Zones | Automatic Failover | Best For |
|----------|-------------|-------------------|-------------------|-------------------|----------|
| Single VM | Fortinet FortiGate VM Next-Generation Firewall - Single | 1 | 1 | No | Development, testing, or non-critical workloads |
| Active/Passive HA - Single Zone | Fortinet FortiGate VM Next-Generation Firewall - A/P HA | 2 | 1 | Yes | Production workloads requiring zone-level redundancy |
| Active/Passive HA - Cross Zone | Fortinet FortiGate VM Next-Generation Firewall - Cross Zone A/P HA | 2 | 2 | Yes | Production workloads requiring the highest availability |
{: caption="Deployment topology comparison" caption-side="bottom"}

Key differences between the topologies:

- **Single VM** — Deploys one FortiGate instance with a public interface (`port1`) and a private interface (`port2`). There is no redundancy. If the instance fails, traffic is interrupted until it is restarted or replaced.
- **Active/Passive HA - Single Zone** — Deploys two FortiGate instances in the same availability zone as an active-passive cluster. The IBM Cloud SDN connector enables automatic failover between nodes. If the active node fails, the passive node takes over without manual intervention.
- **Active/Passive HA - Cross Zone** — Extends the single-zone HA topology across two availability zones. In addition to automatic failover, a public address range enables the floating IP to move between zones, providing resilience against a full zone outage. This is the highest-availability configuration.

For details on what IBM applies to each topology at provisioning time, see [Understanding the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration).

## Choosing a license plan
{: #choosing-a-license-plan}

Select a license plan based on your workload requirements, performance needs, and scale. For more information about FortiGate Security Bundle features, see the [FortiGate Security Bundles page](https://www.fortinet.com/support/support-services/fortiguard-security-subscriptions/fortigate-security-bundles){: external}.

| License Plan | Best For | Supported Sizes | Profile Family | Key Characteristics |
|--------------|----------|------------------|----------------|--------------------|
| ATP (Advanced Threat Protection) | Entry-level deployments | Small, Medium | gen2-cx | Lower cost, limited scale |
| UTP (Unified Threat Protection) | General-purpose security | Small, Medium, Large | gen3-cx | Balanced cost and performance |
| Enterprise | High-performance environments | Medium, Large, X-large | gen3-cx | Highest scalability and throughput |
{: caption="License plan comparison" caption-side="bottom"}

VDOM support is available only with the Enterprise license plan at the X-large (32 vCPU) deployment size.
{: note}

## License plan feature entitlements
{: #license-plan-features}

The following table shows the security services and features included in each license plan.

| Feature | ATP | UTP | Enterprise |
|---------|-----|-----|------------|
| Intrusion Prevention System (IPS) | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Application control | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Geo IP updates | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Advanced Malware Protection (AMP) | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Antivirus | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Botnet protection | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Device and OS detection | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Internet Service (SaaS) database | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Web and content filtering | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Secure DNS filtering | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Video filtering | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| AntiSpam | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| IoT query service | | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| OT protocol service | | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Security Fabric rating and compliance monitoring | | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| AI-based inline malware prevention | | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
{: caption="License plan feature entitlements" caption-side="bottom"}

AI-based inline malware prevention is included with the Enterprise license and can be configured, used, and logged. However, access to FortiGate Cloud and FortiCloud is not available because the license is managed through the IBM Fortinet account.
{: note}

## Sizing and scaling characteristics
{: #sizing-and-scaling-characteristics}

Understanding the sizing and scaling characteristics helps you select the appropriate deployment size for your workload requirements.

### Deployment sizes
{: #deployment-size-definitions}

Deployment sizes are mapped to vCPU allocations.

| Deployment Size | vCPU |
|----------------|------|
| Small | 2 |
| Medium | 8 |
| Large | 16 |
| X-large | 32 |
{: caption="Deployment size vCPU allocations" caption-side="bottom"}

Larger deployment sizes include increased memory, which improves session handling and overall scalability.
{: note}

### Supported sizes by license plan
{: #supported-deployment-sizes-by-license-plan}

The available deployment sizes vary by license plan.

| License Plan | Small (2 vCPU) | Medium (8 vCPU) | Large (16 vCPU) | X-large (32 vCPU) |
|--------------|----------------|------------------|------------------|--------------------|
| Enterprise | Not available | Supported | Supported | Supported |
| UTP | Supported | Supported | Supported | Not available |
| ATP | Supported | Supported | Not available | Not available |
{: caption="Supported deployment sizes by license plan" caption-side="bottom"}

VDOM support is available only with the Enterprise license plan at the X-large (32 vCPU) deployment size.
{: note}

### Session scaling
{: #session-scaling}

Connection capacity increases with deployment size and available memory.

- Smaller deployments support fewer concurrent sessions.
- Larger deployments support significantly higher connection volumes and session tables.

## Instance profile details
{: #instance-profile-details}

Instance profiles define the compute resources allocated to your firewall deployment.

Each license plan includes a specific number of vCPUs for your FortiGate virtual firewall (vFSA). Based on the selected license plan and deployment size, IBM automatically assigns a virtual server instance profile that provides the required vCPUs and throughput. Because available profiles vary by region, IBM manages profile selection and you cannot choose or override the assigned profile.

Virtual server profiles are automatically assigned during provisioning based on the selected license plan and deployment size. You cannot manually select or override the instance profile during deployment.
{: note}

### Enterprise profile mappings
{: #enterprise-profile-mappings}

This license plan uses gen3-cx profiles and supports Medium, Large, and X-large deployment sizes.

| Deployment Size | vCPU | Instance Profile | Profile Family |
|----------------|------|------------------|----------------|
| Medium         | 8    | cx3d-8x20        | gen3-cx        |
| Large          | 16   | cx3d-16x40       | gen3-cx        |
| X-large        | 32   | cx3d-32x80       | gen3-cx        |
{: caption="Enterprise license plan virtual server profile mappings" caption-side="bottom"}

The X-large (32 vCPU) Enterprise deployment is the only configuration that supports virtual domains (VDOMs) and includes 8 VDOMs.
{: note}

### UTP profile mappings
{: #utp-profile-mappings}

This license plan uses gen3-cx profiles and supports Small, Medium, and Large deployment sizes.

| Deployment Size | vCPU | Instance Profile | Profile Family |
|----------------|------|------------------|----------------|
| Small          | 2    | cx3d-2x5         | gen3-cx        |
| Medium         | 8    | cx3d-8x20        | gen3-cx        |
| Large          | 16   | cx3d-16x40       | gen3-cx        |
{: caption="Unified Threat Protection (UTP) license plan virtual server profile mappings" caption-side="bottom"}

### ATP profile mappings
{: #atp-profile-mappings}

This license plan uses gen2-cx profiles and supports Small and Medium deployment sizes. Gen2-cx profiles are not available in all regions; the `cx2-2x4` and `cx2-8x16` profiles are not available in Mumbai, Chennai, and Montreal.

| Deployment Size | vCPU | Instance Profile | Profile Family |
|----------------|------|------------------|----------------|
| Small          | 2    | cx2-2x4          | gen2-cx        |
| Medium         | 8    | cx2-8x16         | gen2-cx        |
{: caption="Advanced Threat Protection (ATP) license plan virtual server profile mappings" caption-side="bottom"}

### Profile considerations
{: #instance-profile-considerations}

Consider the following factors when evaluating instance profiles for your deployment:

- `cx` profiles provide a balanced cost-to-performance ratio and are suitable for most workloads.
- Profiles with higher memory ratios can benefit environments with high session counts.
- Profiles with `-d` include additional instance storage, which can increase cost.
- Profile selection impacts both performance characteristics and pricing.
- Gen3 profiles use newer infrastructure and are recommended for most deployments.
- Gen2 profiles are typically used for smaller or entry-level workloads.

## Related links
{: #license-plans-related-links}

- [Deploying a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-deploy-single-vm)
- [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan)
