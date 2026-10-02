---

copyright:
  years: 2026

lastupdated: "2026-10-02"

keywords: FortiGate backup, FortiGate restore, export configuration, import configuration, FortiGate config backup

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Backing up and restoring the FortiGate configuration
{: #backup-restore-fortigate-config}

Back up your FortiGate VM configuration regularly to protect against data loss and to simplify migration between deployments. You can back up and restore the configuration by using the FortiGate web console or the CLI.
{: shortdesc}

IBM does not back up your FortiGate VM configuration. You are responsible for maintaining backups and storing them securely.
{: important}

## Before you begin
{: #backup-restore-prereqs}

Make sure that the following conditions are met before you backup or restore:

- Be logged in to the FortiGate web console as an administrator. See [Accessing your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall).
- If you are restoring a configuration to a different instance, make sure that the target instance is running the same or a compatible FortiOS version.
- If you are restoring a configuration from a different deployment type (for example, from a Classic FortiGate or a different VPC instance), you must update interface names, IP addresses, and gateway references before you import. See [Adapting a configuration for a new deployment](#adapt-config).

## Backing up the FortiGate configuration in the console
{: #backup-web-console}
{: ui}

To back up the FortiGate configuration in the console, follow these steps:

1. Log in to the FortiGate web console.
1. In the upper-right, click the admin username and select **Configuration > Backup**.
1. In **Backup to**, select **Local PC**.
1. If VDOMs are enabled, select whether to back up the **Global** configuration, a specific VDOM, or all VDOMs.
1. (Optional) Enable **Encrypt configuration file** and enter a password to protect the backup file.
1. Click **Backup**.

The configuration file is downloaded to your local system as a `.conf` file. Store it securely, as the file contains sensitive information including interface configurations, firewall policies, and VPN settings.
{: note}

## Backing up the FortiGate configuration from the CLI
{: #backup-cli}
{: cli}

To back up the configuration from the CLI, run the following command over SSH:

```sh
execute backup config tftp <filename> <tftp-server-ip>
```
{: pre}

Replace `<filename>` with the wanted backup file name and `<tftp-server-ip>` with the IP address of your TFTP server. To back up to a USB drive (if supported by the instance type), use `execute backup config usb <filename>` instead.

## Restoring the FortiGate configuration in the console
{: #restore-web-console}
{: ui}

To restore the FortiGate configuration in the console, follow these steps:

1. Log in to the FortiGate web console.
1. In the upper-right, click the admin username and select **Configuration > Restore**.
1. In **Restore from**, select **Local PC**.
1. Click **Browse** and select the `.conf` backup file.
1. If the backup was encrypted, enter the password.
1. If VDOMs are enabled, select the scope to restore (**Global**, a specific VDOM, or all VDOMs).
1. Click **Restore**.

The FortiGate VM restarts automatically after the restore completes. Log in again when the instance is back online to verify the configuration.

Restoring a configuration overwrites the current running configuration. Make sure that you have a backup of the current configuration before you restore.
{: important}

## Restoring the FortiGate configuration from the CLI
{: #restore-cli}
{: cli}

To restore the configuration from the CLI, run the following command over SSH:

```sh
execute restore config tftp <filename> <tftp-server-ip>
```
{: pre}

Replace `<filename>` with the backup file name and `<tftp-server-ip>` with the IP address of your TFTP server.

## Adapting a configuration for a new deployment
{: #adapt-config}

If you are restoring a configuration to a different FortiGate VM instance (for example, when you change license plans or migrating from Classic to VPC), you must update these items before you import:

- **Interface names**: VPC FortiGate VM deployments use `port1` and `port2`. Classic deployments can use different interface names. Update all references.
- **IP addresses and subnets**: Replace all interface IPs, static routes, and gateway addresses with the values that correspond to the new VPC subnets.
- **HA settings**: If you move from a single VM to an HA deployment, or between HA configurations, update the HA peer IP addresses, management interface, and heartbeat interface settings.
- **SDN connector**: Update the IBM Cloud SDN connector API key and region to match the new deployment.

Edit the `.conf` file in a text editor before you import it. Search for the interface names and IP addresses from the original deployment and replace them with the correct values for the target deployment.
{: tip}

## Unsupported backup methods
{: #unsupported-backup-methods}

**IBM Cloud VPC boot volume snapshots and whole-volume backups are not supported** for FortiGate VM licensed firewall instances.

Do not use VPC volume snapshots to back up or restore a firewall instance for the following reasons:

- **Licensing incompatibility**: FortiOS registers a FortiFlex license specific to the original virtual server instance. A boot volume restored from a snapshot retains the previous instance's license registration, and FortiOS cannot automatically detect or acquire a new license on the new instance.
- **Bypassed deployment automation**: Deploying directly from a snapshot bypasses the Terraform and `cloud-init` automation required to configure instance metadata, licensing services, and associated VPC networking resources.

To back up and restore your firewall, export the configuration `.conf` file as described in [Backing up the FortiGate VM configuration in the console](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config&interface=ui#backup-web-console). If you need to replace an instance, deploy a new firewall from the IBM Cloud catalog and import your configuration file.

## Related links
{: #backup-restore-related-links}

- [Resizing a firewall virtual server instance](/docs/licensed-firewall?topic=licensed-firewall-changing-firewall-instance-profile-or-license-plan)
- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices)
