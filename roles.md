---

copyright:
  years: 2026
lastupdated: "2026-03-27"

subcollection: licensed-firewall

keywords: fortigate, responsibilities, shared responsibilities, licensed firewall, vpc, security

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when using licensed firewall for FortiGate
{: #fortigate-responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use the licensed firewall for FortiGate. For a high-level view of the service types in IBM Cloud and the breakdown of responsibilities between the customer and IBM for each type, see [Shared responsibilities for IBM Cloud offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for IBM when you use the licensed firewall for FortiGate. For the overall terms of use, see [IBM Cloud Terms and Notices](/docs/overview?topic=overview-terms).

## Incident and operations management
{: #fortigate-incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

|  | IBM Responsibilities | Your Responsibilities |
|----------|-------------------------|--------|
| Licensing | Automatically provision and manage FortiGate licenses through the Platform License Manager and VNF License Service. Monitor license status and ensure proper activation during deployment. | Verify license activation after deployment. Select appropriate VPC profile that matches performance requirements. |
| Instance provisioning | Provide VPC infrastructure and automated deployment through IBM Cloud Marketplace. Create Software CRN (SWCRN) for each deployment. Enable Instance Metadata Service for license retrieval. | Configure deployment parameters including network settings, credentials, and resource specifications. Ensure metadata service is enabled on instances. |
| Monitoring and alerting | Provide platform-level monitoring for VPC infrastructure and compute resources. Monitor license service availability and integration with FortiFlex. | Monitor FortiGate application performance, security events, and traffic patterns. Configure FortiGate-specific monitoring and alerting. Review FortiGate logs for security and operational insights. |
| High availability | Provide underlying VPC infrastructure with multi-zone support. Ensure license service availability and redundancy. | Design and implement FortiGate high availability configurations. Configure failover policies and test failover scenarios. |
| Backup and recovery | Maintain backups of VPC infrastructure and platform services. | Back up FortiGate configurations, firewall rules, and policies. Implement disaster recovery procedures for FortiGate instances. Test configuration restoration procedures. |
{: row-headers}
{: caption="Responsibilities for incident and operations" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsibility for. The second column describes IBM responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Change management
{: #fortigate-change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-------------------------|--------|
| Deployment | Provide automated deployment through IBM Cloud Marketplace. Handle license provisioning via cloud-init and metadata service. Integrate with FortiFlex for license management. | Initiate FortiGate deployments. Select appropriate PayGo license plan. Provide deployment configuration including VPC, subnets, and security groups. |
| Configuration | Provide base VPC infrastructure configuration. Ensure proper network connectivity for license activation. | Configure FortiGate firewall rules, policies, NAT, VPN tunnels, and routing. Adapt configurations when migrating from Classic to VPC. Update interface mappings and IP addresses for VPC environment. |
| Updates and patches | Maintain VPC platform and underlying infrastructure. Update license service integrations as needed. | Apply FortiGate software updates and security patches. Test updates in non-production environments before applying to production. |
| Scaling | Provide ability to deploy additional instances or modify VPC profiles. Automatically adjust licensing based on selected profile. | Plan capacity requirements. Deploy additional FortiGate instances as needed. Modify VPC profiles to match performance requirements (VCPU, memory, bandwidth). |
| Decommissioning | Remove license allocations when instances are deleted. Clean up platform resources. | Power off and delete FortiGate instances. Verify traffic has been redirected before decommissioning. Confirm billing has stopped for deleted resources. |
{: row-headers}
{: caption="Responsibilities for change management" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Identity and access management
{: #fortigate-iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-------------------------|--------|
| Platform access | Provide IBM Cloud IAM for platform resource management. Control access to VPC resources and deployment capabilities. | Manage IBM Cloud IAM policies for users and service IDs. Grant appropriate permissions for FortiGate deployment and management. |
| FortiGate access | Provide secure access to FortiGate administrative console through VPC networking. | Manage FortiGate administrative credentials. Configure FortiGate user accounts and role-based access control. Implement multi-factor authentication for FortiGate access. Regularly rotate administrative passwords. |
| Service integration | Provide secure integration between Platform License Manager, VNF License Service, and FortiFlex. | Configure service-to-service authorizations as needed for deployments. |
| Audit logging | Provide IBM Cloud Activity Tracker for platform-level actions. | Enable and review FortiGate audit logs. Monitor administrative access and configuration changes. |
{: row-headers}
{: caption="Responsibilities for identity and access management" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #fortigate-security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-------------------------|--------|
| Infrastructure security | Secure VPC infrastructure and platform services. Protect license service communications and FortiFlex integration. Encrypt data in transit between platform services. | Configure FortiGate security policies and firewall rules. Implement network segmentation and access controls. Enable encryption for VPN tunnels and sensitive traffic. |
| Compliance | Maintain compliance certifications for IBM Cloud platform. Ensure license management processes meet contractual obligations. | Ensure FortiGate configurations meet organizational compliance requirements. Implement security controls required by industry regulations. Document security policies and procedures. |
| Vulnerability management | Patch VPC infrastructure and platform services. Notify customers of security advisories affecting FortiGate. | Apply FortiGate security patches and updates. Scan for vulnerabilities in FortiGate configurations. Respond to security advisories and CVEs. |
| Data protection | Encrypt platform data at rest and in transit. Protect license service data. | Encrypt sensitive data passing through FortiGate. Configure data loss prevention policies. Implement appropriate logging and data retention policies. |
{: row-headers}
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #fortigate-disaster-recovery}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-------------------------|--------|
| DR infrastructure | Provide multi-zone VPC infrastructure for high availability. Ensure license service availability across regions. | Design and implement FortiGate disaster recovery architecture. Deploy FortiGate instances across multiple availability zones. |
| Configuration backup | Maintain backups of VPC infrastructure configuration. | Export and back up FortiGate configurations regularly. Store configuration backups in secure, redundant locations. Document configuration dependencies and requirements. |
| Failover procedures | Provide platform capabilities for multi-zone deployments. Ensure license portability across zones and regions. | Configure and test FortiGate failover procedures. Implement automated failover where possible. Document and practice disaster recovery runbooks. |
| Recovery testing | Test platform disaster recovery capabilities. | Regularly test FortiGate configuration restoration. Validate failover procedures and recovery time objectives (RTO). Conduct disaster recovery drills. |
| Traffic redirection | Provide VPC routing capabilities for traffic management. | Update DNS, routing, or load balancer configurations to redirect traffic during failover. Monitor traffic flows during and after failover events. |
{: row-headers}
{: caption="Responsibilities for disaster recovery" caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Additional considerations
{: #fortigate-additional-considerations}

### License management
{: #fortigate-license-management}

- IBM automatically provisions FortiGate licenses based on the selected VPC profile during deployment
- Licenses are managed through the Platform License Manager and VNF License Service integration with FortiFlex
- No manual license upload is required; the system handles licensing automatically via cloud-init and Instance Metadata Service
- Customers must ensure the Instance Metadata Service is enabled on FortiGate instances for proper license retrieval

### Migration from Classic to VPC
{: #fortigate-migration-considerations}

When migrating from Classic FortiGate to VPC PayGo:

- Customer is responsible for exporting Classic configurations and adapting them for VPC
- Interface names, IP addresses, subnets, and gateway references must be updated to match VPC environment
- Customer must select VPC profiles that match or exceed Classic FortiGate performance (VCPU, memory, bandwidth)
- IBM handles license provisioning for new VPC instances; customer manages configuration migration
- Customer should validate connectivity and test failover before redirecting production traffic

For detailed migration guidance, see [Migrating Fortinet FortiGate from Classic to VPC PayGo](/docs/licensed-firewall?topic=licensed-firewall-tutorial-fortigate-vpc-migration).

### Performance and capacity planning
{: #fortigate-capacity-planning}

- Customer is responsible for selecting appropriate VPC profiles based on performance requirements
- Refer to [Fortinet datasheets](https://www.fortinet.com/resources/datasheets){: external} for FortiGate performance specifications
- IBM provides the infrastructure; customer must monitor and adjust capacity as needed
- Consider multi-zone deployments for high availability and increased capacity

### Support and troubleshooting
{: #fortigate-support}

- IBM provides support for VPC infrastructure, platform services, and license provisioning
- Customer is responsible for FortiGate application-level configuration and troubleshooting
- For FortiGate-specific issues, refer to [Fortinet documentation](https://docs.fortinet.com/){: external}
- IBM Support can assist with platform-level issues and license service problems
