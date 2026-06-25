---

copyright:
  years: 2026

lastupdated: "2026-06-25"

keywords: resize firewall, change license, vsi resize, firewall migration

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading the FortiGate software
{: #upgrading-fortigate-software}

Use the native **Fortinet Fabric Upgrade** tool to upgrade or downgrade the FortiGate virtual firewall software. The Fabric Upgrade tool is the recommended and supported method for managing firmware versions because it automatically handles configuration backup and restoration during the firmware version change process.
{: shortdesc}

## Before you begin
{: #upgrading-fortigate-software-prereqs}

Before upgrading the FortiGate software:

- Verify that you have administrative access to the FortiGate Web Console.
- Review the target firmware version and any applicable release notes.
- Schedule a maintenance window because the FortiGate appliance restarts during the firmware upgrade.
- Ensure that network traffic can tolerate a brief service interruption.

## Upgrading the firmware
{: #upgrading-firmware}

The **Fortinet Fabric Upgrade** tool is the primary method for upgrading and downgrading the vFSA firmware version. This tool is owned and supported by Fortinet.

To upgrade the firmware:

1. Log in to the FortiGate Web Console.
2. Navigate to **System > Firmware & Registration > Fabric Upgrade**.
3. Review the list of available firmware versions.
4. Select the target firmware version.
5. Click **Upgrade** to begin the firmware installation.

During the upgrade process, the Fabric Upgrade tool:

- Downloads a backup of the current configuration to your local system.
- Installs the selected firmware version.
- Automatically restores the saved configuration after the firmware installation completes.
- Restarts the FortiGate appliance to complete the upgrade.

## Downgrading the firmware
{: #downgrading-firmware}

If you need to revert to an earlier supported firmware version, use the same **Fabric Upgrade** tool.

To downgrade the firmware:

1. Log in to the FortiGate Web Console.
2. Navigate to **System > Firmware & Registration > Fabric Upgrade**.
3. Select a supported earlier firmware version.
4. Click **Downgrade**.
5. Wait for the appliance to restart and restore the configuration.

## Verifying the upgrade
{: #verifying-firmware-upgrade}

After the appliance restarts:

1. Log back in to the FortiGate Web Console.
2. Verify that the expected firmware version is installed.
3. Confirm that the firewall configuration has been restored successfully.
4. Validate network connectivity and firewall services before returning the system to production.

## Next steps
{: #upgrading-fortigate-software-next-steps}

For detailed information about supported upgrade paths, firmware compatibility, and troubleshooting, see the Fortinet documentation for the Fabric Upgrade tool.
