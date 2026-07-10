---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-23"

keywords: fortinet, vfsa, fortigate, security appliance, racii, licensing, support

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# IBM-Fortinet RACII v3
{: #fortinet-racii-v3}

[PLACEHOLDER][: tag-purple}  **RABINDRA: DO WE EVEN NEED THIS IN CUSTOMER-FACING DOCS? HAVE PROVIDED ALTERNATIVE IN TOPIC ABOVE.**

Service: Fortinet vFSA (Virtual FortiGate Security Appliance) in IBM Cloud VPC
{: shortdesc}

## README & Legend
{: #readme-legend}

| Item | Description |
|------|-------------|
| **Purpose** | RACII between IBM, Fortinet (Vendor), and Customer for Fortinet vFSA licensing, support, and lifecycle management in IBM Cloud VPC |
| **Scope** | Fortinet vFSA (Virtual FortiGate Security Appliance) deployed as customer-managed virtual server instances in IBM Cloud VPC, licensed via FortiFlex licensing platform. |
| **IBM Organization** | IBM encompasses multiple teams: IBM Support (technical support), IBM License team (Network License Provider and PaaS License Manager), IBM Cloud VPC (infrastructure platform). All represented collectively as "IBM" in this RACII. |
| **Service Model** | **IBM Licensed Service with Support** - IBM provides license procurement and management through FortiFlex, plus technical support as single point of contact. Customers deploy and manage their own vFSA virtual server instances in their IBM Cloud accounts. Fortinet provides product images, updates, and TAC resolution. |
| **Support Model** | **IBM Support acts as triage and coordination layer** - Customers open all technical tickets with IBM Support. IBM Support performs L1/L2 triage and opens Fortinet TAC cases when needed. IBM Support manages all Fortinet interactions on behalf of customers. |
| **Key Distinction** | This is NOT a managed service. IBM does not deploy or configure customer vFSA instances. IBM provides license management and support coordination. Customers own and operate their vFSA infrastructure. |
{: caption="Table 1. Service overview" caption-side="bottom"}

### Legend
{: #legend}

- **R** = Responsible (does the work)
- **A** = Accountable (owns the outcome)
- **C** = Consulted (provides input)
- **I** = Informed (kept updated)

## A. Support
{: #support}

| Activity | IBM | Fortinet (Vendor) | Customer (Virtual Server Owner) |
|----------|-----|-------------------|----------------------|
| L1/L2 incident triage & restoration | R/A | I | I |
| L3 complex troubleshooting | C | R/A | I |
| Vendor TAC case creation/management | R | C | I |
| Root Cause Analysis (RCA) to customer | R/A | C | I |
| Knowledge base (runbooks, known errors) | R/A | C | I |
| Event monitoring & alerting | I | I | R/A |
| Major Incident (P1) management & comms | R/A | C | I |
{: caption="Table 2. Support responsibilities" caption-side="bottom"}

### Notes
{: #support-section-notes}

- Customers open support tickets with IBM Support for all vFSA technical issues
- IBM Support performs initial triage and troubleshooting
- IBM Support opens and manages Fortinet TAC cases when needed
- IBM Support acts as single point of contact between customer and Fortinet

## B. Maintenance
{: #maintenance}

| Activity | IBM | Fortinet (Vendor) | Customer (Virtual Server Owner) |
|----------|-----|-------------------|----------------------|
| Software/firmware upgrades (vFSA images) | I | R/A (publish) | R/A (apply to virtual servers) |
| Signature/IPS/AV/URL updates | I | R/A (publish) | R/A (configure/apply) |
| Virtual server lifecycle (start/stop/resize/delete) | I | I | R/A |
| IBM Cloud VPC infrastructure maintenance | R/A | I | I |
| Capacity & health assessments | R/A | C (product guidance) | I |
| EoL/EoS lifecycle management | R (communicate to customers) | R/A (publish notices) | R/A (plan & execute) |
| Backup & config baseline enforcement | I | I | R/A |
{: caption="Table 3. Maintenance responsibilities" caption-side="bottom"}

### Notes
{: #maintenance-section-notes}

- IBM communicates Fortinet EoL/EoS notices to license holders
- Customers are responsible for all vFSA configuration and operational changes
- IBM manages underlying VPC platform infrastructure maintenance

## C. Security Fix
{: #security-fix}

| Activity | IBM | Fortinet (Vendor) | Customer (Virtual Server Owner) |
|----------|-----|-------------------|----------------------|
| Vulnerability disclosure & PSIRT advisory | I | R/A | I |
| Patch development & hotfix build | I | R/A | I |
| Patch testing | I | R/A | I |
| Production patch deployment | I | I | R/A |
| Emergency fix (zero-day/actively exploited) | R (communicate urgency) | R/A (publish fix) | R/A (apply fix) |
| Secure config baselines (CIS/NIST) | I | R/A (publish) | R/A (implement) |
{: caption="Table 4. Security fix responsibilities" caption-side="bottom"}

### Notes
{: #security-section-notes}

- IBM monitors Fortinet PSIRT advisories and communicates critical issues to license holders
- IBM has no ability to publish images, modify vFSA configurations, or deploy patches
- Customers are responsible for applying all security updates to their virtual servers

## D. SLA
{: #sla}

| Activity | IBM | Fortinet (Vendor) | Customer (Virtual Server Owner) |
|----------|-----|-------------------|----------------------|
| FortiFlex platform availability (license provisioning) | R (monitor & escalate) | R/A (operate platform) | I |
| vFSA data plane availability | I | I | R/A |
| Vendor TAC SLO (response/engagement) | I | R/A | I |
| License provisioning SLA (IBM to customer) | R/A | R/A (FortiFlex dependency) | I |
| SLA breach management (license-related) | R/A | C | I |
| Compliance, audit & evidence (product) | I | R/A | C |
| Compliance, audit & evidence (deployment) | I | I | R/A |
| Monthly license status reporting | R/A | I | I |
| Monthly service reporting (vFSA operations) | I | I | R/A |
{: caption="Table 5. SLA responsibilities" caption-side="bottom"}

### Notes
{: #sla-section-notes}

- IBM's SLA covers FortiFlex license provisioning only, not vFSA operational availability
- FortiFlex platform downtime impacts license provisioning (control plane) but not existing vFSA data plane operations
- Customers are responsible for their own vFSA operational SLAs
- Fortinet TAC SLOs apply to technical support cases

## E. Licensing (RACII)
{: #licensing}

| Activity | IBM | Fortinet (Vendor) | Customer (Virtual Server Owner) |
|----------|-----|-------------------|----------------------|
| License procurement & quoting (new/expansion) | R/A | C (price list/guidance) | I |
| License renewal management & co-termination | R/A | C (contract options) | I |
| Entitlement tracking & compliance | R/A | C (back-end records) | I |
| Contract registration & asset binding (SN.contract) | R/A | C | I |
| Activation & subscription sync (FortiFlex) | R | R/A (FortiFlex platform) | I |
| License health monitoring (expiry, gaps, coverage) | R/A | I | I |
| Support tier management (upgrade/downgrade) | R/A (advise & execute) | C (options/SKU) | I |
| License transfer/rehost (virtual server replacement) | R (coordinate & validate) | R/A (enable in FortiFlex) | R (request) |
| True-up/audit support (proof of entitlement) | R/A | C | I |
| Budget forecasting & cost allocation | R/A | I | C |
| EULA/compliance & record retention | R/A | C | I |
| Monthly license status reporting (KPIs) | R/A | I | I |
{: caption="Table 6. Licensing responsibilities" caption-side="bottom"}

### Notes
{: #licensing-section-notes}

- IBM acts as License Provider/Reseller of Record
- All license operations flow through FortiFlex platform
- Customers request license changes through IBM

## F. Escalation & Decision Scenarios
{: #escalation-scenarios}

### Scenario 1: Incident Flow (Suspected Product Defect)
{: #scenario-incident}

| Party | Actions |
|-------|---------|
| **Customer** | Open support ticket with IBM Support; provide initial diagnostics; implement workarounds as advised |
| **IBM Support** | Perform L1/L2 triage; troubleshoot; open Fortinet TAC case if needed; coordinate resolution; update customer |
| **Fortinet** | Provide diagnostics, workarounds, or hotfix to IBM Support; advise on risks |
{: caption="Table 7. Incident flow" caption-side="bottom"}

IBM Support acts as single point of contact. Customer does not open TAC cases directly with Fortinet. IBM Support manages all Fortinet TAC interactions.

### Scenario 2: Emergency Fix (Critical PSIRT / Zero-Day)
{: #scenario-emergency}

| Party | Actions |
|-------|---------|
| **Fortinet** | Publish PSIRT advisory; provide mitigation guidance to IBM Support; deliver hotfix/patch with ETA |
| **IBM Support** | Monitor PSIRT feeds; communicate critical advisories to all license holders; provide guidance on mitigations; coordinate patch deployment support |
| **Customer** | Evaluate risk; implement mitigations with IBM Support guidance; apply patches to VSIs; manage change control |
{: caption="Table 8. Emergency fix flow" caption-side="bottom"}

IBM Support provides guidance and coordination but cannot modify customer vFSA configurations. Customers apply patches to their own VSIs.

### Scenario 3: Hardware/Infrastructure Issue
{: #scenario-infrastructure}

| Party | Actions |
|-------|---------|
| **Customer** | Open support ticket with IBM Support; provide diagnostics |
| **IBM Support** | Triage to determine if virtual server, vFSA software, or underlying infrastructure issue; coordinate with appropriate teams |
| **IBM Cloud Infrastructure** | Resolve underlying VPC platform issues (hypervisor, network, storage) if escalated by IBM Support |
| **Fortinet** | Provide guidance to IBM Support if vFSA software is impacted by infrastructure behavior |
{: caption="Table 9. Infrastructure issue flow" caption-side="bottom"}

IBM Support performs initial triage and routes to appropriate team (IBM Cloud Infrastructure or Fortinet TAC).

### Scenario 4: Lifecycle (EoL/EoS)
{: #scenario-lifecycle}

| Party | Actions |
|-------|---------|
| **Fortinet** | Publish EoL/EoS notices; provide compatibility matrices and migration guidance to IBM Support; set support end dates |
| **IBM Support** | Communicate Fortinet notices to affected license holders; provide license upgrade/migration options; assist with migration planning; track customer plans |
| **Customer** | Assess impact with IBM Support; plan migration/upgrade; execute changes in their environment; manage change control |
{: caption="Table 10. Lifecycle management flow" caption-side="bottom"}

IBM Support facilitates license transitions and provides migration guidance. Customers execute technical changes to their VSIs.

### Scenario 5: FortiFlex Platform Outage
{: #scenario-outage}

| Party | Actions |
|-------|---------|
| **Fortinet** | Restore FortiFlex platform; communicate status and ETA to IBM Support; provide workarounds if available |
| **IBM Support** | Monitor FortiFlex status; escalate to Fortinet; communicate to customers; coordinate workarounds; track impact |
| **Customer** | Informed of outage by IBM Support; existing vFSA instances continue operating (data plane unaffected) |
{: caption="Table 11. Platform outage flow" caption-side="bottom"}

FortiFlex outage impacts new license provisioning only. Existing licensed vFSA instances continue normal operation. IBM Support manages all communication.

## G. Update & Change Flow
{: #update-flow}

### Normal Update Flow
{: #normal-update}

1. Fortinet publishes new vFSA image/update
1. IBM Support notifies license holders
1. Customer evaluates and plans deployment
1. Customer applies update to their virtual servers
1. Customer validates and closes change

### Emergency/Critical Update Flow
{: #emergency-update}

1. Fortinet publishes critical PSIRT advisory
1. IBM Support immediately notifies all license holders
1. IBM Support provides mitigation guidance
1. Customer implements mitigations
1. Fortinet delivers hotfix/patch
1. IBM Support coordinates patch deployment support
1. Customer applies patch to virtual servers
1. Customer validates and reports back to IBM Support

### Key Principles
{: #update-principles}

- IBM Support acts as single point of contact for all technical issues
- IBM Support provides guidance but does not deploy updates to customer virtual servers
- IBM Support coordinates with Fortinet TAC when needed
- Customers own all technical implementation decisions and change control

## H. Assumptions & Scope
{: #assumptions}

| Area | Assumption | Notes |
|------|------------|-------|
| **Operating Model** | IBM Licensed Service (customer-managed) | NOT a fully-managed service. Customers deploy and operate vFSA in their own IBM Cloud accounts. |
| **Fortinet Support Tier** | Premium/Priority TAC available | Actual TAC tier depends on customer's license SKU. IBM can facilitate tier upgrades. |
| **Regulatory** | General enterprise controls | Customers responsible for compliance in their deployments (PCI/SOX/HIPAA/ISO). Fortinet provides product compliance documentation. |
| **Tooling** | FortiFlex as licensing platform | No FortiManager or FortiAnalyzer in standard VPC offering. Customers may deploy these separately if needed. |
| **Multi-vendor** | Fortinet vFSA primary focus | Integration with other security tools (SIEM/SOAR) is customer responsibility. |
| **License Provider** | IBM acts as License Provider/Reseller | IBM manages procurement, renewals, entitlements, and FortiFlex relationship. |
| **Infrastructure** | Customer-owned virtual servers in IBM Cloud VPC | Customers manage their virtual server lifecycle. IBM Cloud Infrastructure team (separate) manages underlying VPC platform. |
| **Support Relationship** | Customer → IBM Support → Fortinet TAC | Customers open tickets with IBM Support. IBM Support triages and opens Fortinet TAC cases when needed. |
{: caption="Table 12. Assumptions and scope" caption-side="bottom"}

## I. Out of Scope
{: #out-of-scope}

The following are explicitly NOT covered by this RACII:

- **IBM Cloud VPC Platform Operations** - Managed by IBM Cloud Infrastructure team (separate organization)
- **Customer Application/Workload Support** - Customer responsibility
- **Network Design & Architecture** - Customer responsibility (Fortinet provides reference architectures)
- **Performance Tuning** - Customer responsibility (IBM Support coordinates with Fortinet TAC for guidance)
- **Custom Integrations** - Customer responsibility
- **Third-party Tools** - Unless specifically part of Fortinet stack
- **Training & Enablement** - Available separately from Fortinet and IBM

## J. Document Control
{: #document-control}

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| v1 | [Original Date] | [Author] | Initial version - assumed fully-managed service model |
| v2 | [v2 Date] | [Author] | Updated to "Licensed Service" model; corrected several R/A assignments |
| v3 | 2026-02-23 | [Author] | PROPOSED - Addressed all feedback gaps; clarified scope; added FortiFlex SLA; removed contradictions; added update flow; expanded assumptions |
{: caption="Table 13. Document version history" caption-side="bottom"}
