---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate security, firewall best practices, security group, admin access, FortiGate hardening, management access, least privilege

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Security best practices for FortiGate on IBM Cloud VPC
{: #fortigate-security-best-practices}

Follow these security best practices to reduce the attack surface of your FortiGate deployment, protect management access, and maintain a strong security posture throughout the lifecycle of your firewall.
{: shortdesc}

## Restrict management access through the security group
{: #bp-restrict-management-access}

Every FortiGate deployment creates a dedicated security group that denies all inbound management traffic. When you open the security group, you will see a small number of pre-configured inbound rules. These exist solely to allow HA cluster nodes to communicate with each other and to reach licensing services. They do not permit management access and must not be removed. When you open management access, follow these principles:

- **Allow only your administrator IP addresses.** Add inbound TCP rules for port 443 (HTTPS) or port 22 (SSH) with a specific source IP address or CIDR range. Do not use `0.0.0.0/0` as the source.
- **Use the narrowest CIDR possible.** If your administrators connect from a known IP range, restrict the source to that range only.
- **Separate management access from data plane traffic.** If possible, access the FortiGate management interface from a dedicated management subnet or through a VPN, rather than directly over the public floating IP.
- **Review the security group rules regularly.** Remove any inbound rules that are no longer needed, such as rules added for temporary access.

For step-by-step instructions, see [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).

## Change the default administrator password immediately
{: #bp-change-default-password}

The initial administrator password is generated at deployment time and is displayed in the IBM Cloud Schematics workspace output. This password is visible to anyone with access to the Schematics workspace.

- Change the administrator password the first time you log in to the FortiGate web console.
- Use a strong, unique password that is not shared with other systems.
- Store the password in a secrets manager or password vault rather than in plain text.

## Disable unused management protocols on each FortiGate interface
{: #bp-disable-unused-protocols}

Within the FortiGate itself, the default bootstrap configuration enables HTTPS, SSH, and ping on both `port1` and `port2`. This is independent of the IBM Cloud security group, which controls traffic at the VPC network layer. For additional hardening, you can use the FortiGate web console to disable any management protocols on each interface that are not required for your operational workflow.

1. Log in to the FortiGate web console.
1. Go to **Network > Interfaces**.
1. Edit `port1` and `port2`.
1. In **Administrative access**, uncheck any protocols that are not in use.
1. Click **OK** to save.

Leaving ping (`PING`) enabled on the public interface (`port1`) allows external hosts to probe the firewall's presence. Disable it if internet-facing discovery is a concern.
{: tip}

## Use a VPN or bastion host for management access
{: #bp-vpn-management}

Where possible, avoid exposing the FortiGate management interface directly on the public floating IP. Instead:

- Deploy a VPN termination point or a bastion host in your VPC.
- Access the FortiGate management interface from within the VPC over a private IP address.
- Restrict the management security group rule to the VPN or bastion host source IP only.

This approach eliminates direct internet-facing management access entirely.

## Keep FortiOS firmware up to date
{: #bp-firmware-updates}

Running a supported and patched firmware version is one of the most effective defenses against known vulnerabilities.

- Subscribe to Fortinet PSIRT advisories to be notified of new vulnerabilities. See [Keeping abreast of firmware updates and vulnerability patches](/docs/licensed-firewall?topic=licensed-firewall-addressing-vulnerabilities).
- Review the FortiGate release notes before upgrading to understand any behavior changes.
- Schedule firmware upgrades during a maintenance window. The FortiGate restarts during an upgrade.
- For HA deployments, follow Fortinet's recommended upgrade sequence to minimize downtime.

For upgrade instructions, see [Keeping abreast of firmware updates and vulnerability patches](/docs/licensed-firewall?topic=licensed-firewall-addressing-vulnerabilities).

## Enable logging for all firewall policies
{: #bp-enable-logging}

Ensure that traffic logging is enabled on your firewall policies, especially for allowed traffic. Without logging, you have no record of what traffic passed through the firewall.

1. Log in to the FortiGate web console.
1. Go to **Policy & Objects > Firewall Policy**.
1. Edit each policy and ensure that **Log Allowed Traffic** is set to **All Sessions** or at minimum **Security Events**.
1. Click **OK** to save.

## Apply security profiles to internet-facing policies
{: #bp-security-profiles}

For any firewall policy that allows traffic from the internet or to untrusted networks, attach security profiles to inspect the traffic:

- **IPS** — Detects and blocks known attack patterns.
- **Antivirus** — Scans file transfers for malware.
- **Web filtering** — Controls access to website categories (UTP and Enterprise tiers).
- **Application control** — Identifies and enforces policy on applications (UTP and Enterprise tiers).

For instructions, see [Enabling security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services).

## Limit administrator accounts and use role-based access
{: #bp-admin-accounts}

- Do not share the `admin` account between multiple administrators. Create named administrator accounts with the minimum privileges required for each role.
- Use read-only profiles for monitoring accounts.
- Review administrator accounts periodically and remove accounts that are no longer needed.

## Back up your configuration regularly
{: #bp-configuration-backup}

IBM does not back up your FortiGate configuration. You are responsible for maintaining backups.

- Export the configuration from the FortiGate web console on a regular schedule. For step-by-step instructions, see [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config).
- Store the backup securely and off the FortiGate instance (for example, in IBM Cloud Object Storage).
- Test configuration restore procedures before you need them in production.

## Related links
{: #security-best-practices-related-links}

- [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall)
- [Enabling security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services)
- [Keeping abreast of firmware updates and vulnerability patches](/docs/licensed-firewall?topic=licensed-firewall-addressing-vulnerabilities)
- [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config)
