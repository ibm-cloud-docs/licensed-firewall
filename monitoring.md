---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate monitoring, firewall logs, FortiGate metrics, IBM Cloud Monitoring, fortigate health, traffic monitoring

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring your FortiGate firewall
{: #monitoring}

Monitor the health, performance, and traffic of your FortiGate firewall to detect issues and validate that policies are working as expected.
{: shortdesc}

## Viewing FortiGate logs and events
{: #monitoring-fortigate-logs}

FortiGate generates logs for traffic, events, security threats, and system activity. You can view these logs directly in the FortiGate web console.

1. Log in to the FortiGate web console.
1. Go to **Log & Report**.
1. Select the log category you want to review:

   | Category | Description |
   |----------|-------------|
   | **Traffic** | All traffic flows processed by firewall policies, including allowed and denied connections. |
   | **Event** | System events such as logins, configuration changes, and HA state changes. |
   | **Security** | Threats detected by security profiles such as IPS, antivirus, and web filtering. |
   | **VPN** | VPN tunnel establishment, authentication, and traffic. |
   {: caption="FortiGate log categories" caption-side="bottom"}

## Monitoring instance health in IBM Cloud
{: #monitoring-instance-health}

You can monitor the underlying virtual server instance that runs your FortiGate from the IBM Cloud console.

1. In the [IBM Cloud console](/login), click the navigation menu and select **Infrastructure > Compute > Virtual server instances**.
1. Click the FortiGate virtual server instance to open its Details page.
1. Review the **Activity** and **Monitoring** tabs for CPU, memory, and network metrics.

For deeper observability, you can connect the instance to IBM Cloud Monitoring. For more information, see [Getting started with IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started).

## Checking firewall policy hit counts
{: #monitoring-policy-hits}

Policy hit counts show how often each firewall policy is matching traffic. Reviewing hit counts helps you identify unused policies or unexpected traffic patterns.

1. Log in to the FortiGate web console.
1. Go to **Policy & Objects > Firewall Policy**.
1. Review the **Bytes** and **Sessions** columns to see traffic volumes per policy.

Policies with zero hits over an extended period may be candidates for review or removal.
{: tip}

## Monitoring interface and routing status
{: #monitoring-interface-status}

1. Log in to the FortiGate web console.
1. Go to **Network > Interfaces** to review the status and traffic statistics for `port1` and `port2`.
1. Go to **Network > Routing** to verify that routing tables are correct and that the default gateway is reachable.

## Troubleshooting connectivity
{: #monitoring-troubleshoot-connectivity}

If traffic is not flowing as expected, use the following FortiGate built-in tools:

- **Packet capture** — Go to **Network > Diagnostics > Packet Capture** to capture traffic on a specific interface.
- **Debug flow** — Use the FortiGate CLI command `diagnose debug flow` to trace traffic through the policy engine.
- **Ping and traceroute** — Go to **Network > Diagnostics** to run ping or traceroute from the FortiGate to a destination.

## Related links
{: #monitoring-related-links}

- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices)
- [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support)
