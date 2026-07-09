---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: FortiGate IPS, antivirus, web filtering, application control, security profiles, FortiGate security services, intrusion prevention

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Enabling security services
{: #enable-security-services}

FortiGate security services extend basic firewall policies with deep traffic inspection capabilities. Available services depend on your license tier.
{: shortdesc}

Security services are applied to traffic by attaching security profiles to firewall policies. For detailed instructions on creating and applying security profiles, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.

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
