---

copyright:
  years: 2026

lastupdated: "2026-08-05"

keywords: FortiGate configuration, firewall policy, configure FortiGate, initial setup, routing, security profiles, FortiGate web console

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Configuring your FortiGate firewall after deployment
{: #configuring-fortigate}

After you log in to the FortiGate web console for the first time, the firewall is running with its default bootstrap configuration. The bootstrap configuration initializes the system settings required for IBM Cloud integration, but it does not include any firewall policies. No traffic is inspected or permitted through the firewall until you create policies.
{: shortdesc}

Before making changes, review [Understanding the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration) to understand what IBM applied during provisioning. Use this topic as a starting checklist for the configuration tasks you should complete before putting the firewall into production.

## Change the administrator password
{: #config-change-password}

The initial administrator password is displayed in your IBM Cloud Schematics workspace output and is visible to anyone with access to that workspace. Change it immediately on first login.

1. Log in to the FortiGate web console.
1. In the top-right corner, click the **admin** username and select **Change Password**.
1. Enter a strong, unique password and confirm it.
1. Click **OK**.

Store the new password in a secrets manager or password vault. Do not store it in plain text.
{: important}

## Verify the network interfaces
{: #config-verify-interfaces}

Confirm that the network interfaces reflect the IP addressing you configured during deployment. The bootstrap configuration pre-assigns IPs based on your Schematics input variables for HA deployments. Single VM deployments use DHCP.

1. Log in to the FortiGate web console.
1. Go to **Network > Interfaces**.
1. Verify that `port1` (public), `port2` (private), and — for HA deployments — `port3` (HA heartbeat) and `port4` (HA management) show the expected IP addresses and aliases.

For guidance on interface configuration, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.

## Configure routing
{: #config-routing}

The FortiGate requires static routes to direct traffic correctly between interfaces. At minimum, you need a default route on `port1` pointing to the public subnet gateway so that outbound internet traffic — including license validation and FortiGuard updates — can flow.

For inter-subnet or internet-bound traffic to flow through the firewall, you must also update the VPC routing tables to send traffic to the FortiGate's private interface (`port2`) as the next hop. For step-by-step instructions, see [Step 3: Route traffic through the firewall](/docs/licensed-firewall?topic=licensed-firewall-access-firewall#access-firewall-routing).

For guidance on static route configuration on the FortiGate itself, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.

## Create firewall policies
{: #config-firewall-policies}

The bootstrap configuration does not include any firewall policies. Without at least one allow policy, the FortiGate drops all traffic passing between interfaces, even if VPC routing is correctly configured.

Create firewall policies that match your traffic requirements. At minimum, consider:

- An outbound policy from `port2` (private) to `port1` (public) to allow workloads to reach the internet.
- An inbound policy from `port1` (public) to `port2` (private) for any services you are exposing.
- Inter-subnet policies if you are routing traffic between VPC subnets through the firewall.

For step-by-step guidance on creating firewall policies, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.

## Enable security profiles
{: #config-security-profiles}

Attach security profiles to your firewall policies to activate threat inspection on traffic that is permitted through the firewall. The profiles available to you depend on your license plan.

For instructions on enabling Intrusion Prevention System (IPS), antivirus, web filtering, and application control, see [Enabling security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services).

## Adjust management access per interface
{: #config-management-access}

The default bootstrap configuration enables HTTPS, SSH, and ping on both `port1` and `port2`. Review and restrict these settings to match your operational requirements.

1. Log in to the FortiGate web console.
1. Go to **Network > Interfaces**.
1. Edit each interface and adjust **Administrative access** to allow only the protocols needed on that interface.
1. Click **OK** to save.

For security hardening recommendations, see [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices).

## Back up your initial configuration
{: #config-initial-backup}

After completing your initial configuration, create a backup before the firewall enters production. This gives you a clean restore point.

For backup instructions, see [Backing up and restoring the FortiGate configuration](/docs/licensed-firewall?topic=licensed-firewall-backup-restore-fortigate-config).

## Related links
{: #configuring-fortigate-related-links}

- [Understanding the default firewall configuration](/docs/licensed-firewall?topic=licensed-firewall-understanding-default-firewall-configuration)
- [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}
