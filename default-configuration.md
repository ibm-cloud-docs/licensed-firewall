---

copyright:
  years: 2026

lastupdated: "2026-07-15"

keywords: firewall default configuration, FortiGate default config, HA configuration, bootstrap configuration, SDN connector, public address range, cloud-init

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Understanding the default firewall configuration
{: #understanding-default-firewall-configuration}

Each FortiGate firewall offering is deployed with a default bootstrap configuration that IBM Cloud applies during provisioning. The bootstrap configuration initializes the firewall with the required system settings before the instance becomes available.
{: shortdesc}

Depending on the deployment model, the bootstrap configuration configures:

- System hostname
- Network interfaces, interface aliases, and management access
- High availability (HA) settings
- IBM Cloud SDN connector integration
- Public Address Range integration for cross-zone deployments

After deployment, you can modify the default configuration to meet your networking and security requirements.

## Deployment comparison
{: #deployment-comparison}

The following table summarizes how the three deployment models differ.

| Feature | Single VM | HA single-zone | HA cross-zone |
| ------- | --------- | -------------- | ------------- |
| Firewall instances | 1 | 2 | 2 |
| Availability zones | 1 | 1 | 2 |
| High availability | No | Yes | Yes |
| HA heartbeat | No | Yes | Yes |
| Dedicated HA management interface | No | Yes | Yes |
| IBM Cloud SDN connector | No | Yes | Yes |
| Public Address Range | No | No | Yes |
| Automatic failover | No | Yes | Yes |
{: caption="Deployment model comparison" caption-side="bottom"}

The bootstrap configuration differs across the following deployment models:

- [Single VM](#single-vm-default-config)
- [HA single-zone — active node](#ha-single-zone-active-node)
- [HA single-zone — passive node](#ha-single-zone-passive-node)
- [HA cross-zone — active node](#ha-cross-zone-active-node)
- [HA cross-zone — passive node](#ha-cross-zone-passive-node)

Bootstrap configurations contain variables that are replaced with deployment-specific values at provisioning time. For a complete list of variables and their sources, see [Bootstrap variables](#bootstrap-variables).

Every deployment creates a dedicated security group. By default, all inbound traffic is denied except the traffic that is required for HA communication. Before you can access the firewall by using HTTPS or SSH, add inbound security group rules that allow management access from trusted IP addresses.
{: important}

## Default configuration of a Single VM deployment
{: #single-vm-default-config}

A Single VM deployment provisions one FortiGate firewall with one public interface (`port1`) and one private interface (`port2`). The bootstrap configuration is static. It contains no dynamic variables and applies the same defaults to every Single VM deployment.

```text
config system global
    set hostname FGT-IBM
end

config system interface
    edit port1
        set alias untrust
        set allowaccess https ssh ping
    next
    edit port2
        set alias trust
        set allowaccess https ssh ping
    next
end
```
{: codeblock}

The following table describes the default interface settings:

| Interface | Alias | Management access |
| --------- | ----- | ----------------- |
| `port1` | `untrust` | HTTPS, SSH, ping |
| `port2` | `trust` | HTTPS, SSH, ping |
{: caption="Single VM default interface configuration" caption-side="bottom"}

## Default configuration of an HA single-zone deployment — active node
{: #ha-single-zone-active-node}

An HA single-zone deployment provisions two FortiGate firewalls within the same availability zone as an active-passive cluster. The active and passive nodes receive separate bootstrap configurations.

The active node is assigned a higher HA priority (`50`), which ensures it becomes the primary firewall after deployment. It also receives the IBM Cloud SDN connector configuration, which enables automatic failover.

```text
config system global
    set hostname IBM-HA-Active
end

config system interface
    edit port2
        set ip ${fgt_1_static_port2} ${netmask}
    next
    edit port3
        set ip ${fgt_1_static_port3} ${netmask}
    next
    edit port4
        set ip ${fgt_1_static_port4} ${netmask}
    next
end

config system ha
    set mode a-p
    set priority 50
    set unicast-hb-peerip ${fgt_2_static_port3}
    set password ${ha_password}
    config ha-mgmt-interfaces
        edit 1
            set interface port4
            set gateway ${fgt1_port_4_mgmt_gateway}
        next
    end
end

config system sdn-connector
    edit "ibm"
        set api-key ${ibm_api_key}
        set region ${region}
    next
end
```
{: codeblock}

The following table describes the default settings for the active node:

| Setting | Value | Description |
| ------- | ----- | ----------- |
| Hostname | `IBM-HA-Active` | Identifies the active node. |
| HA mode | `a-p` | Configures active-passive HA. |
| HA priority | `50` | Higher priority ensures this node is the active firewall. |
| HA heartbeat peer | `${fgt_2_static_port3}` | IP address of the passive node heartbeat interface. |
| HA management interface | `port4` | Dedicated out-of-band management port for HA. |
| IBM Cloud SDN connector | `ibm` | Enables IBM Cloud integration for automatic failover. |
{: caption="HA single-zone active node default settings" caption-side="bottom"}

## Default configuration of an HA single-zone deployment — passive node
{: #ha-single-zone-passive-node}

The passive node is assigned a lower HA priority (`25`), which keeps it in the standby role during normal operation. It continuously synchronizes configuration and session state from the active node and automatically takes over if the active node fails.

```text
config system global
    set hostname IBM-HA-Passive
end

config system interface
    edit port2
        set ip ${fgt_2_static_port2} ${netmask}
    next
    edit port3
        set ip ${fgt_2_static_port3} ${netmask}
    next
    edit port4
        set ip ${fgt_2_static_port4} ${netmask}
    next
end

config system ha
    set mode a-p
    set priority 25
    set unicast-hb-peerip ${fgt_1_static_port3}
    set password ${ha_password}
    config ha-mgmt-interfaces
        edit 1
            set interface port4
            set gateway ${fgt2_port_4_mgmt_gateway}
        next
    end
end

config system sdn-connector
    edit "ibm"
        set api-key ${ibm_api_key}
        set region ${region}
    next
end
```
{: codeblock}

The following table describes the default settings for the passive node:

| Setting | Value | Description |
| ------- | ----- | ----------- |
| Hostname | `IBM-HA-Passive` | Identifies the passive node. |
| HA mode | `a-p` | Configures active-passive HA. |
| HA priority | `25` | Lower priority keeps this node in the standby role. |
| HA heartbeat peer | `${fgt_1_static_port3}` | IP address of the active node heartbeat interface. |
| HA management interface | `port4` | Dedicated out-of-band management port for HA. |
| IBM Cloud SDN connector | `ibm` | Enables IBM Cloud integration for automatic failover. |
{: caption="HA single-zone passive node default settings" caption-side="bottom"}

## Default configuration of an HA cross-zone deployment — active node
{: #ha-cross-zone-active-node}

An HA cross-zone deployment provisions two FortiGate firewalls across separate availability zones. The cross-zone configuration extends the single-zone HA configuration with two additions: a public address range identifier in the SDN connector for cross-zone floating IP failover, and a VDOM exception list that ensures interfaces, static routes, and virtual IPs are synchronized between nodes.

```text
config system global
    set hostname IBM-HA-Active
end

config system interface
    edit port2
        set ip ${fgt_1_static_port2} ${netmask}
    next
    edit port3
        set ip ${fgt_1_static_port3} ${netmask}
    next
    edit port4
        set ip ${fgt_1_static_port4} ${netmask}
    next
end

config system ha
    set mode a-p
    set priority 50
    set unicast-hb-peerip ${fgt_2_static_port3}
    set password ${ha_password}
    config ha-mgmt-interfaces
        edit 1
            set interface port4
            set gateway ${fgt1_port_4_mgmt_gateway}
        next
    end
end

config system sdn-connector
    edit "ibm"
        set par-id ${par_id}
        set api-key ${ibm_api_key}
        set region ${region}
    next
end

config system vdom-exception
    edit 1
        set object system.interface
    next
    edit 2
        set object router.static
    next
    edit 3
        set object firewall.vip
    next
end
```
{: codeblock}

The following table describes the default settings for the active node:

| Setting | Value | Description |
| ------- | ----- | ----------- |
| Hostname | `IBM-HA-Active` | Identifies the active node. |
| HA mode | `a-p` | Configures active-passive HA. |
| HA priority | `50` | Higher priority ensures this node is the active firewall. |
| HA heartbeat peer | `${fgt_2_static_port3}` | IP address of the passive node heartbeat interface (Zone 2). |
| HA management interface | `port4` | Dedicated out-of-band management port for HA. |
| Public Address Range | `${par_id}` | Enables floating IP failover across availability zones. |
| IBM Cloud SDN connector | `ibm` | Enables IBM Cloud integration. |
| VDOM exceptions | `system.interface`, `router.static`, `firewall.vip` | Objects synchronized independently from the HA cluster sync. |
{: caption="HA cross-zone active node default settings" caption-side="bottom"}

## Default configuration of an HA cross-zone deployment — passive node
{: #ha-cross-zone-passive-node}

The passive node in a cross-zone deployment mirrors the active node configuration with a lower HA priority and reversed peer IP references. It includes the same public address range–enabled SDN connector and VDOM exception list as the active node.

```text
config system global
    set hostname IBM-HA-Passive
end

config system interface
    edit port2
        set ip ${fgt_2_static_port2} ${netmask}
    next
    edit port3
        set ip ${fgt_2_static_port3} ${netmask}
    next
    edit port4
        set ip ${fgt_2_static_port4} ${netmask}
    next
end

config system ha
    set mode a-p
    set priority 25
    set unicast-hb-peerip ${fgt_1_static_port3}
    set password ${ha_password}
    config ha-mgmt-interfaces
        edit 1
            set interface port4
            set gateway ${fgt2_port_4_mgmt_gateway}
        next
    end
end

config system sdn-connector
    edit "ibm"
        set par-id ${par_id}
        set api-key ${ibm_api_key}
        set region ${region}
    next
end

config system vdom-exception
    edit 1
        set object system.interface
    next
    edit 2
        set object router.static
    next
    edit 3
        set object firewall.vip
    next
end
```
{: codeblock}

The following table describes the default settings for the passive node:

| Setting | Value | Description |
| ------- | ----- | ----------- |
| Hostname | `IBM-HA-Passive` | Identifies the passive node. |
| HA mode | `a-p` | Configures active-passive HA. |
| HA priority | `25` | Lower priority keeps this node in the standby role. |
| HA heartbeat peer | `${fgt_1_static_port3}` | IP address of the active node heartbeat interface (Zone 1). |
| HA management interface | `port4` | Dedicated out-of-band management port for HA. |
| Public Address Range | `${par_id}` | Enables floating IP failover across availability zones. |
| IBM Cloud SDN connector | `ibm` | Enables IBM Cloud integration. |
| VDOM exceptions | `system.interface`, `router.static`, `firewall.vip` | Objects synchronized independently from the HA cluster sync. |
{: caption="HA cross-zone passive node default settings" caption-side="bottom"}

## Bootstrap variables
{: #bootstrap-variables}

Bootstrap configurations contain variables that IBM Cloud replaces with deployment-specific values at provisioning time. These values come from inputs that you provide when you order the firewall, or from resources that IBM Cloud creates automatically during deployment.

The following table lists all bootstrap variables and their sources:

| Variable | Source | Description |
| -------- | ------ | ----------- |
| `${fgt_1_static_port2}` | `FGT1_STATIC_IP_PORT2` order input | Private interface IP for FortiGate 1. |
| `${fgt_1_static_port3}` | `FGT1_STATIC_IP_PORT3` order input | HA heartbeat IP for FortiGate 1. |
| `${fgt_1_static_port4}` | `FGT1_STATIC_IP_PORT4` order input | HA management IP for FortiGate 1. |
| `${fgt_2_static_port2}` | `FGT2_STATIC_IP_PORT2` order input | Private interface IP for FortiGate 2. |
| `${fgt_2_static_port3}` | `FGT2_STATIC_IP_PORT3` order input | HA heartbeat IP for FortiGate 2. |
| `${fgt_2_static_port4}` | `FGT2_STATIC_IP_PORT4` order input | HA management IP for FortiGate 2. |
| `${fgt1_port_4_mgmt_gateway}` | `FGT1_PORT4_MGMT_GATEWAY` order input | Gateway for FortiGate 1 HA management subnet. |
| `${fgt2_port_4_mgmt_gateway}` | `FGT2_PORT4_MGMT_GATEWAY` order input | Gateway for FortiGate 2 HA management subnet. |
| `${ha_password}` | Derived internally by IBM Cloud | HA cluster authentication password. |
| `${ibm_api_key}` | `IBMCLOUD_API_KEY` order input | IBM Cloud API key for the SDN connector. |
| `${region}` | `REGION` order input | IBM Cloud region where the firewall is deployed. |
| `${par_id}` | Created automatically by IBM Cloud | Public Address Range identifier (cross-zone only). |
| `${netmask}` | Fixed value | Subnet mask (`255.255.255.0`). |
{: caption="Bootstrap configuration variables" caption-side="bottom"}


## Related links
{: #default-config-related-links}

- [Deploying a licensed FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-fortinet-firewall-order)
- [Accessing the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall)
- [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config)
