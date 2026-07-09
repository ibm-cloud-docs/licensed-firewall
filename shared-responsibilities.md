---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: shared responsibilities, customer responsibilities, IBM responsibilities, FortiGate support, firewall operations, patch management

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Shared responsibilities for FortiGate licensed firewall
{: #shared-responsibilities}

The FortiGate licensed firewall is a customer-managed service. IBM provides license management and technical support coordination, but you are responsible for deploying, configuring, operating, and maintaining your FortiGate virtual server instances. Use this topic to understand where IBM's responsibilities end and yours begin.
{: shortdesc}

## IBM responsibilities
{: #ibm-responsibilities}

IBM is responsible for the following:

- **License management** — Procuring, provisioning, renewing, and tracking FortiGate licenses through the FortiFlex platform. You do not need to manage licenses directly.
- **License health monitoring** — Monitoring license expiry and coverage, and communicating any issues to you.
- **Support coordination** — Acting as the single point of contact for all FortiGate technical issues. IBM Support performs initial triage and opens Fortinet TAC cases on your behalf when needed. You do not open TAC cases directly with Fortinet.
- **Security advisory communications** — Monitoring Fortinet PSIRT advisories and notifying you of critical vulnerabilities and recommended mitigations.
- **End-of-life notifications** — Communicating Fortinet end-of-life and end-of-support notices to you and providing guidance on migration options.
- **IBM Cloud VPC platform** — Maintaining the underlying VPC infrastructure (hypervisor, networking, storage) that your FortiGate virtual server instances run on.

## Your responsibilities
{: #customer-responsibilities}

As the virtual server owner, you are responsible for the following:

- **Deployment and configuration** — Deploying FortiGate instances from the IBM Cloud catalog, configuring firewall policies, network interfaces, routing, and VPN settings.
- **Firewall policy management** — Creating and maintaining all firewall policies and security profiles. IBM does not configure or modify your firewall policies.
- **Firmware and software updates** — Applying FortiGate firmware updates and security patches to your virtual server instances. IBM notifies you of critical updates but does not apply them.
- **Security patching** — Evaluating Fortinet PSIRT advisories, implementing recommended mitigations, and applying patches during your own change control process.
- **Configuration backup** — Backing up your FortiGate configuration regularly. IBM does not back up your configuration.
- **Monitoring and alerting** — Monitoring the health, performance, and traffic of your FortiGate instances. See [Monitoring your FortiGate firewall](/docs/licensed-firewall?topic=licensed-firewall-monitoring).
- **Virtual server lifecycle** — Starting, stopping, resizing, and deleting your FortiGate virtual server instances.
- **Compliance** — Ensuring your deployment meets applicable regulatory and compliance requirements (for example, PCI, HIPAA, ISO).

## Getting support
{: #shared-responsibilities-support}

Open all FortiGate support cases with IBM Support. IBM Support performs initial triage and opens a Fortinet TAC case on your behalf when the issue requires Fortinet involvement. Do not open TAC cases directly with Fortinet.

For more information, see [Getting help and support](/docs/licensed-firewall?topic=licensed-firewall-help-and-support).

### What to expect when you open a support case
{: #support-case-flow}

**For general technical issues:**
1. You open a support case with IBM Support and provide initial diagnostics.
1. IBM Support performs triage and troubleshooting.
1. If the issue requires Fortinet involvement, IBM Support opens a Fortinet TAC case and manages all Fortinet interactions on your behalf.
1. IBM Support provides you with updates and coordinates the resolution.

**For critical security vulnerabilities (PSIRT / zero-day):**
1. Fortinet publishes a PSIRT advisory.
1. IBM Support notifies you and provides mitigation guidance.
1. You evaluate the risk and implement mitigations in your environment.
1. When Fortinet delivers a patch, IBM Support coordinates support for your deployment.
1. You apply the patch to your virtual server instances and validate the fix.

**For end-of-life or end-of-support notices:**
1. Fortinet publishes end-of-life notices.
1. IBM Support communicates the notices to you and provides license upgrade or migration options.
1. You plan and execute the migration or upgrade in your environment.

### What IBM Support cannot do
{: #support-limitations}

- IBM Support cannot modify your FortiGate configuration, firewall policies, or network settings.
- IBM Support cannot apply firmware updates or security patches to your virtual server instances.
- IBM Support cannot access your FortiGate management interface.

## Summary
{: #shared-responsibilities-summary}

| Responsibility | IBM | You |
|----------------|-----|-----|
| License provisioning and management | ✓ | |
| License health monitoring and renewal | ✓ | |
| Security advisory notifications | ✓ | |
| IBM Cloud VPC platform maintenance | ✓ | |
| Support triage and TAC coordination | ✓ | |
| FortiGate deployment and configuration | | ✓ |
| Firewall policy management | | ✓ |
| Firmware and security patch application | | ✓ |
| Configuration backup | | ✓ |
| Monitoring and alerting | | ✓ |
| Virtual server lifecycle management | | ✓ |
| Compliance and regulatory controls | | ✓ |
{: caption="Shared responsibilities summary" caption-side="bottom"}
