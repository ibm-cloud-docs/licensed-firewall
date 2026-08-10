---

copyright:
  years: 2026
lastupdated: "2026-08-10"

keywords: FortiGate IPS, antivirus, web filtering, application control, security profiles, FortiGate security services, intrusion prevention, deep packet inspection, SSL inspection, TLS inspection

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Enabling security services
{: #enable-security-services}

FortiGate security services extend basic firewall policies with deep traffic inspection capabilities. Available services depend on your license tier.
{: shortdesc}

Security services are applied to traffic by attaching security profiles to firewall policies. A firewall policy must exist before you can attach a security profile to it. For instructions on creating firewall policies, see [Configuring your FortiGate firewall after deployment](/docs/licensed-firewall?topic=licensed-firewall-configuring-fortigate).

## Available security services by license tier
{: #security-services-by-tier}

| Service | ATP | UTP | Enterprise |
|---------|-----|-----|------------|
| Intrusion Prevention (IPS) | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Antivirus | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Web Filtering | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| DNS Filtering | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
| Application Control | | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") | ![Checkmark icon](../icons/checkmark-icon.svg "Checkmark") |
{: caption="Security services available by license tier" caption-side="bottom"}

Enabling multiple security services on the same firewall policy increases CPU usage and may reduce throughput. Size your deployment accordingly. For more information, see [About firewall license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles).
{: note}

## Deep packet inspection
{: #deep-packet-inspection}

Security services such as IPS, antivirus, web filtering, and application control require deep packet inspection (DPI) to examine the contents of network traffic. For encrypted traffic (HTTPS and other TLS-based protocols), FortiGate must perform SSL/TLS inspection to decrypt, inspect, and re-encrypt the traffic before it reaches its destination.

Without SSL/TLS inspection enabled, FortiGate can only inspect unencrypted traffic. Security profiles attached to firewall policies will not detect threats or enforce policies within encrypted sessions.

FortiGate supports two SSL inspection modes:

- **Certificate inspection** — Inspects the certificate presented during the TLS handshake without decrypting the payload. This is a lighter-weight option that can identify the destination and enforce basic controls, but cannot detect threats hidden inside encrypted content.
- **Full SSL inspection** — Decrypts, inspects, and re-encrypts traffic. This enables all security services to operate on encrypted traffic, providing the highest level of protection. It requires deploying the FortiGate CA certificate to client devices so that they trust the re-signed certificates.

For step-by-step instructions on configuring SSL/TLS inspection, see the [FortiGate SSL inspection documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/255100/ssl-tls-inspection-overview){: external}.

## Enabling security profiles
{: #enabling-security-profiles}

Security profiles are created and then attached to firewall policies to apply inspection to matching traffic flows.

### Intrusion Prevention System (IPS)
{: #enable-ips}

IPS monitors traffic for known attack signatures and anomalies and can block or log matching traffic.

1. In the FortiGate web console, go to **Security Profiles > Intrusion Prevention**.
1. Create a new IPS sensor or modify the default sensor.
1. Add signatures relevant to your environment and set the action to **Block** or **Monitor**.
1. Attach the IPS sensor to your firewall policy under **Security Profiles > IPS**.

For more information, see the [FortiGate IPS documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}.

### Antivirus
{: #enable-antivirus}

Antivirus scanning inspects file transfers for malware in supported protocols (HTTP, HTTPS with SSL inspection, FTP, SMTP, and others).

1. In the FortiGate web console, go to **Security Profiles > AntiVirus**.
1. Create or modify an antivirus profile and set the action for infected files.
1. Attach the profile to your firewall policy under **Security Profiles > AntiVirus**.

For more information, see the [FortiGate antivirus documentation](https://docs.fortinet.com/search?q=antivirus&p=fortigate){: external}.

### Web filtering
{: #enable-web-filtering}

Web filtering controls access to websites based on categories, URLs, and content ratings.

1. In the FortiGate web console, go to **Security Profiles > Web Filter**.
1. Create or modify a web filter profile, enabling or blocking categories as required.
1. Attach the profile to your firewall policy under **Security Profiles > Web Filter**.

For more information, see the [FortiGate web filtering documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}.

### Application control
{: #enable-application-control}

Application control identifies and controls applications regardless of port or protocol, using Fortinet's application signature database.

1. In the FortiGate web console, go to **Security Profiles > Application Control**.
1. Create or modify an application control profile and set actions for application categories.
1. Attach the profile to your firewall policy under **Security Profiles > Application Control**.

For more information, see the [FortiGate application control documentation](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}.

## Related links
{: #security-services-related-links}

- [FortiGate Administration Guide](https://docs.fortinet.com/document/fortigate/latest/administration-guide/954635/getting-started){: external}
- [FortiGate SSL/TLS inspection overview](https://docs.fortinet.com/document/fortigate/latest/administration-guide/255100/ssl-tls-inspection-overview){: external}
- [Security best practices for FortiGate on IBM Cloud VPC](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices)
- [Monitoring your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-monitoring)
- [About firewall license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles)
