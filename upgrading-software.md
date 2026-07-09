---

copyright:
  years: 2026

lastupdated: "2026-07-15"

keywords: upgrade fortigate, downgrade fortigate, firmware update, fortigate software, fabric upgrade, fortios

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Upgrading the FortiGate software
{: #upgrading-fortigate-software}

Use the native Fortinet Fabric Upgrade tool to upgrade or downgrade the FortiGate virtual firewall software. The Fabric Upgrade tool is the supported method for managing firmware versions for the IBM-licensed firewall.
{: shortdesc}

## Before you begin
{: #upgrading-fortigate-software-prereqs}

Before upgrading the FortiGate software:

- Verify that you have administrator access to the FortiGate Web Console.
- Review the release notes for the target firmware version.
- Verify the supported upgrade path if upgrading across multiple FortiOS releases.
- Schedule a maintenance window because the FortiGate restarts during the upgrade.
- Verify that the appliance can access FortiGuard to download firmware.

For more information, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.

## Upgrade the firmware
{: #upgrading-firmware}

To upgrade the firmware:

1. Log in to the FortiGate Web Console.
2. Go to **System > Firmware & Registration**.
3. Click **Fabric Upgrade**.
4. Select either the **Latest** or **All Upgrades** tab.
5. Select the target firmware version.
6. If the selected firmware requires one or more intermediate builds, choose one of the following options:
   - **Follow the recommended upgrade path** to allow FortiGate to automatically install each required firmware version and restart as needed.
   - **Upgrade directly** to install the selected firmware version.
7. Confirm the upgrade.

During the upgrade, FortiGate downloads the required firmware from FortiGuard, installs the firmware, restarts as needed, and displays the upgrade status. If the recommended upgrade path is selected, FortiGate automatically performs each intermediate upgrade until the target version is installed.

For more information, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.

## Downgrading the firmware
{: #downgrading-firmware}

To downgrade the firmware:

1. Log in to the FortiGate Web Console.
2. Go to **System > Firmware & Registration**.
3. Click **Fabric Upgrade**.
4. Select the required earlier supported firmware version.
5. Confirm the downgrade.

The FortiGate installs the selected firmware and restarts automatically.

## Verifying the upgrade
{: #verifying-firmware-upgrade}

After the appliance restarts:

1. Log back in to the FortiGate Web Console.
2. Verify that the expected firmware version is installed.
3. Verify that the upgrade completed successfully.
4. Confirm network connectivity before returning the appliance to production.

## Next steps
{: #upgrading-fortigate-software-next-steps}

For more information, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.
