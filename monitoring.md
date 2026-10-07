---

copyright:
  years: 2026

lastupdated: "2026-10-07"

keywords: FortiGate monitoring, firewall logs, FortiGate metrics, IBM Cloud Monitoring, fortigate health, traffic monitoring, VPC flow logs

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring your FortiGate firewall
{: #monitoring}

Monitor the health, performance, and traffic of your FortiGate VM firewall to detect issues and validate that policies are working as expected.
{: shortdesc}

Monitoring responsibilities are split between IBM Cloud infrastructure tooling and the FortiGate management interface. IBM Cloud provides visibility into the underlying virtual server instance health and network flow data. FortiGate VM provides traffic logs, security event logs, and policy-level diagnostics.

## Monitoring instance health in IBM Cloud
{: #monitoring-instance-health}

The FortiGate VM firewall runs as a standard VPC virtual server instance. You can monitor its health, resource utilization, and lifecycle events in the same way as any other virtual server instance. For more information, see [Getting started with IBM Cloud monitoring](/docs/monitoring?topic=monitoring-getting-started).

For HA deployments, check both the active and passive node instances. A passive node that shows high CPU or unexpected restarts might indicate a failover event or a sync issue.
{: tip}

## Monitoring network traffic with VPC Flow Logs
{: #monitoring-flow-logs}

VPC Flow Logs capture metadata about network traffic flowing through your VPC, including traffic to and from the FortiGate interfaces. Flow logs are useful for auditing traffic patterns, investigating incidents, and verifying that routing is directing traffic through the firewall as expected.

Flow logs capture connection metadata (source IP, destination IP, port, protocol, bytes, and action) but do not capture packet payloads.
{: note}

To enable VPC Flow Logs for your FortiGate VM subnets, see [About Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs).

## Monitoring FortiGate logs and events
{: #monitoring-fortigate-logs}

FortiGate VM generates detailed logs for traffic flows, security events, system activity, and VPN sessions. Review these logs in the FortiGate web console under **Log & Report**.

Log categories include traffic logs, security threat logs (IPS, antivirus, web filter), system event logs, and VPN logs. For full details on log types, filtering, and export options, see the [FortiGate logging and reporting documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}.

## Monitoring firewall policy activity
{: #monitoring-policy-hits}

Policy hit counts and session statistics show how often each firewall policy is matching traffic. Reviewing these helps identify unused policies or unexpected traffic patterns.

In the FortiGate web console, go to **Policy & Objects > Firewall Policy** to review bytes and session counts per policy. For more information, see the [FortiGate firewall policy documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}.

## Diagnosing connectivity issues
{: #monitoring-troubleshoot-connectivity}

FortiGate provides built-in diagnostic tools for troubleshooting traffic flows, including packet capture, debug flow tracing, and ping and traceroute. For instructions on using these tools, see the [FortiGate diagnostic tools documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/troubleshooting){: external}.

If you cannot connect to the FortiGate management interface, see [Troubleshooting: Cannot connect to FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-troubleshoot-cannot-connect).

## Related links
{: #monitoring-related-links}

- [FortiGate logging and reporting documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}
- [FortiGate Administration Guide](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}
- [About Flow Logs for VPC](/docs/vpc?topic=vpc-flow-logs)
- [Getting started with IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-getting-started)
- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices)
- [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support)
