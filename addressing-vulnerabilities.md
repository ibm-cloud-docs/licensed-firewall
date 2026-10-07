---

copyright:
  years: 2026

lastupdated: "2026-10-07"

keywords: FortiGate vulnerability, firmware update, PSIRT advisory, FortiOS patch, security advisory, vulnerability management, FortiGate upgrade, Fortinet notifications, fabric upgrade

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Managing firmware updates and vulnerability patches
{: #addressing-vulnerabilities}

Keeping abreast of Fortinet security advisories and applying firmware updates in a timely manner is your responsibility. IBM manages licensing and provides support coordination but does not apply updates to customer-managed FortiGate VM instances on your behalf.
{: shortdesc}

For a full description of customer and IBM responsibilities, see [Shared responsibilities for FortiGate licensed firewall](/docs/licensed-firewall?topic=licensed-firewall-shared-responsibilities).

## Step 1: Subscribing to Fortinet security notifications
{: #vuln-subscribe}

Subscribe to Fortinet notification services to receive alerts when new security advisories, firmware releases, and threat intelligence updates are published.

- **Fortinet PSIRT portal** - Fortinet's Product Security Incident Response Team (PSIRT) publishes advisories for all known vulnerabilities that affect FortiGate products, including severity ratings, affected versions, and recommended remediation actions. Bookmark and review the [Fortinet PSIRT portal](https://www.fortiguard.com/psirt){: external} regularly.
- **RSS feeds** - Subscribe to Fortinet RSS feeds for security advisories, firmware releases, and product announcements. For more information, see [Manage Your Subscriptions to Fortinet](https://www.fortinet.com/rss-feeds){: external}.

Review Fortinet PSIRT advisories regularly and include them in your organization's vulnerability management processes.
{: important}

## Step 2: Assessing the impact on your deployment
{: #vuln-assess}

When a new advisory is published, determine whether your deployment is affected before taking action.

1. Identify the FortiOS version running on your instance. In the FortiGate web console, go to **Dashboard > Status** and note the firmware version that is displayed under **System Information**.
2. Compare your running version against the affected versions listed in the advisory.
3. Review the advisory's CVSS severity score and any available mitigations or workarounds.

For guidance on interpreting FortiGate advisories and understanding severity ratings, see the [Fortinet PSIRT portal](https://www.fortiguard.com/psirt){: external} and the [FortiGate Administration Guide](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}.

## Step 3: Backing up your configuration before patching
{: #vuln-backup}

Before applying any firmware update, back up your FortiGate VM configuration. This action protects you if the upgrade needs to be rolled back.

For step-by-step instructions, see [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config).

## Step 4: Applying the firmware update
{: #vuln-upgrade}

Use the native Fortinet Fabric Upgrade tool to upgrade the FortiGate VM firmware. The Fabric Upgrade tool is the supported method for managing firmware versions for the IBM-licensed firewall. Schedule a maintenance window because the FortiGate VM restarts during the upgrade.

Before you begin:

- Verify that you have administrator access to the FortiGate web console.
- Review the release notes for the target firmware version.
- Verify the supported upgrade path if upgrading across multiple FortiOS releases.
- Verify that the appliance can access FortiGuard to download firmware.

Network requirements:

- The FortiGate VM must have egress internet access to reach Fortinet's update servers. Verify that the VPC has a public gateway attached to the subnet used by `port1`, or that a floating IP is assigned to `port1`.
- HTTPS access to the FortiGate web console on port `443` is required. Ensure that your inbound security group rules allow HTTPS access from your management IP addresses, or use VPN access over a private IP address.

To upgrade the firmware, follow these steps:

1. Log in to the FortiGate web console.
2. Go to **System > Firmware & Registration**.
3. Click **Fabric Upgrade**.
4. Select either the **Latest** or **All Upgrades** tab.
5. Select the target firmware version.
6. If the selected firmware requires one or more intermediate builds, choose one of the following options:
   - **Follow the recommended upgrade path** to allow the FortiGate VM to automatically install each required firmware version and restart as needed.
   - **Upgrade directly** to install the selected firmware version.
7. Confirm the upgrade.

During the upgrade, FortiGate VM downloads the required firmware from FortiGuard, installs it, restarts as needed, and displays the upgrade status.

## Step 5: Verifying and confirming
{: #vuln-verify}

After the upgrade completes and the instance restarts:

1. Log back in to the FortiGate web console.
2. Confirm the firmware version under **Dashboard > Status > System Information**.
3. Verify network connectivity and that firewall policies are operating as expected.
4. Review the advisory to confirm that the installed version is listed as a remediated release.

## Downgrading the firmware
{: #vuln-downgrade}

If you need to roll back to an earlier firmware version:

1. Log in to the FortiGate web console.
2. Go to **System > Firmware & Registration**.
3. Click **Fabric Upgrade**.
4. Select the required earlier supported firmware version.
5. Confirm the downgrade.

The FortiGate VM installs the selected firmware and restarts automatically.

## Related links
{: #addressing-vulnerabilities-related-links}

- [Fortinet PSIRT portal](https://www.fortiguard.com/psirt){: external}
- [Fortinet RSS feeds](https://www.fortinet.com/rss-feeds){: external}
- [FortiGate Administration Guide](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}
- [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config)
- [Shared responsibilities for FortiGate licensed firewall](/docs/licensed-firewall?topic=licensed-firewall-shared-responsibilities)
- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices)
