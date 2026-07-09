---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate backup, FortiGate restore, export configuration, import configuration, FortiGate config backup

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Backing up and restoring the FortiGate configuration
{: #backup-restore-fortigate-config}

Back up your FortiGate configuration regularly to protect against data loss and to simplify migration between deployments. You can back up and restore the configuration using the FortiGate web console or the CLI.
{: shortdesc}

IBM does not back up your FortiGate configuration. You are responsible for maintaining backups and storing them securely.
{: important}

## Before you begin
{: #backup-restore-prereqs}

- You must be logged in to the FortiGate web console as an administrator. See [Accessing the FortiGate web console](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).
- If you are restoring a configuration to a different instance, ensure the target instance is running the same or a compatible FortiOS version.
- If you are restoring a configuration from a different deployment type (for example, from a Classic FortiGate or a different VPC instance), you must update interface names, IP addresses, and gateway references before importing. See [Adapting a configuration for a new deployment](#adapt-config).

## Backing up the configuration
{: #backup-config}

### Back up using the web console
{: #backup-web-console}

To back up the FortiGate configuration from the web console, complete the following steps.

1. Log in to the FortiGate web console.
1. In the top-right corner, click the admin username and select **Configuration > Backup**.
1. Under **Backup to**, select **Local PC**.
1. If VDOMs are enabled, select whether to back up the **Global** configuration, a specific VDOM, or all VDOMs.
1. Optionally, enable **Encrypt configuration file** and enter a password to protect the backup file.
1. Click **Backup**.

The configuration file is downloaded to your local machine as a `.conf` file. Store it securely, as the file contains sensitive information including interface configurations, firewall policies, and VPN settings.
{: note}

### Back up using the CLI
{: #backup-cli}

You can also back up the configuration using the FortiGate CLI over SSH:

```sh
execute backup config tftp <filename> <tftp-server-ip>
```
{: pre}

Replace `<filename>` with the desired backup file name and `<tftp-server-ip>` with the IP address of your TFTP server. To back up to a USB drive (if supported by the instance type), use `execute backup config usb <filename>` instead.

## Restoring the configuration
{: #restore-config}

### Restore using the web console
{: #restore-web-console}

To restore the FortiGate configuration from the web console, complete the following steps.

1. Log in to the FortiGate web console.
1. In the top-right corner, click the admin username and select **Configuration > Restore**.
1. Under **Restore from**, select **Local PC**.
1. Click **Browse** and select the `.conf` backup file.
1. If the backup was encrypted, enter the password.
1. If VDOMs are enabled, select the scope to restore (**Global**, a specific VDOM, or all VDOMs).
1. Click **Restore**.

The FortiGate restarts automatically after the restore completes. Log in again when the instance is back online to verify the configuration.

Restoring a configuration overwrites the current running configuration. Ensure you have a backup of the current configuration before restoring.
{: important}

### Restore using the CLI
{: #restore-cli}

You can also restore the configuration using the FortiGate CLI over SSH:

```sh
execute restore config tftp <filename> <tftp-server-ip>
```
{: pre}

Replace `<filename>` with the backup file name and `<tftp-server-ip>` with the IP address of your TFTP server.

## Adapting a configuration for a new deployment
{: #adapt-config}

If you are restoring a configuration to a different FortiGate instance (for example, when changing license plans or migrating from Classic to VPC), you must update the following before importing:

- **Interface names** — VPC FortiGate deployments use `port1` and `port2`. Classic deployments may use different interface names. Update all references accordingly.
- **IP addresses and subnets** — Replace all interface IPs, static routes, and gateway addresses with the values that correspond to the new VPC subnets.
- **HA settings** — If moving from a single VM to an HA deployment, or between HA configurations, update the HA peer IP addresses, management interface, and heartbeat interface settings.
- **SDN connector** — Update the IBM Cloud SDN connector API key and region to match the new deployment.

Edit the `.conf` file in a text editor before importing it. Search for the interface names and IP addresses from the original deployment and replace them with the correct values for the target deployment.
{: tip}

## Next steps
{: #backup-restore-next-steps}

- [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan) — if you are changing license plans, use this topic for the full procedure including configuration export and import steps.
- [Security best practices](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices) — store backup files in IBM Cloud Object Storage and test restore procedures regularly.
