---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate firewall policy, firewall rules, inbound outbound rules, FortiGate policy, least privilege firewall

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Configuring firewall policies
{: #configure-policies}

Define how traffic flows through your firewall.
{: shortdesc}

FortiGate firewall policies control the traffic that is allowed or denied between network interfaces. Policies are configured in the FortiGate web console and are evaluated in order from top to bottom.

For detailed guidance on creating and managing firewall policies, refer to the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external} on the Fortinet documentation site.

## Key policy concepts
{: #policy-concepts}

- **Source and destination interfaces** — Each policy applies to traffic flowing between a source interface (for example, port1) and a destination interface (for example, port2).
- **Source and destination addresses** — Policies can match specific IP addresses, address ranges, or address objects.
- **Services** — Policies can restrict traffic to specific protocols and port numbers.
- **Action** — Each policy either accepts or denies matching traffic.
- **Security profiles** — Policies can apply security profiles such as IPS, antivirus, and web filtering to accepted traffic.

## Applying least-privilege principles
{: #least-privilege}

IBM recommends configuring firewall policies to allow only the traffic that is explicitly required for your workloads.

- Start with a default-deny posture and add explicit allow rules for required traffic flows.
- Restrict management access (HTTPS, SSH) to known administrator IP addresses.
- Review and remove unused or overly broad policies regularly.
