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

Security services are applied to traffic by attaching **security profiles** to firewall policies. You create profiles for each service, then reference those profiles in the policies where you want the service to be active.

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

## Enabling Intrusion Prevention (IPS)
{: #enable-ips}

IPS inspects traffic for known attack signatures and blocks or alerts on detected threats.

1. Log in to the FortiGate web console.
1. Go to **Security Profiles > Intrusion Prevention**.
1. Click **Create New** to create an IPS profile, or edit an existing profile.
1. Select the signatures and sensor settings appropriate for your environment.
1. Click **OK** to save the profile.
1. Go to **Policy & Objects > Firewall Policy** and open the policy where you want to apply IPS.
1. Under **Security Profiles**, enable **IPS** and select the profile you created.
1. Click **OK** to save the policy.

## Enabling antivirus scanning
{: #enable-antivirus}

Antivirus scanning inspects file transfers within allowed traffic for malware.

1. Log in to the FortiGate web console.
1. Go to **Security Profiles > AntiVirus**.
1. Click **Create New** or edit an existing antivirus profile.
1. Configure the inspection mode and file type settings.
1. Click **OK** to save the profile.
1. Go to **Policy & Objects > Firewall Policy** and open the policy where you want to apply antivirus scanning.
1. Under **Security Profiles**, enable **AntiVirus** and select the profile.
1. Click **OK** to save the policy.

## Enabling web filtering
{: #enable-web-filtering}

Web filtering blocks or monitors access to websites by category, URL, or content rating. Available on UTP and Enterprise license tiers.

1. Log in to the FortiGate web console.
1. Go to **Security Profiles > Web Filter**.
1. Click **Create New** or edit an existing web filter profile.
1. Configure URL filtering categories, block or monitor actions, and safe search settings.
1. Click **OK** to save the profile.
1. Apply the profile to a firewall policy under **Security Profiles > Web Filter**.

## Enabling application control
{: #enable-application-control}

Application control identifies and enforces policies on applications regardless of port or protocol. Available on UTP and Enterprise license tiers.

1. Log in to the FortiGate web console.
1. Go to **Security Profiles > Application Control**.
1. Click **Create New** or edit an existing application control profile.
1. Select application categories or specific applications to block, monitor, or allow.
1. Click **OK** to save the profile.
1. Apply the profile to a firewall policy under **Security Profiles > Application Control**.

Enabling multiple security services on the same firewall policy increases CPU usage and may reduce throughput. Size your deployment accordingly. For more information, see [About firewall license plans and instance profiles](/docs/licensed-firewall?topic=licensed-firewall-about-firewall-license-plans-and-instance-profiles).
{: note}
