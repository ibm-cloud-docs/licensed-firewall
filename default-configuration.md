---

copyright:
  years: 2026

lastupdated: "2026-06-26"

keywords: firewall default configuration, FortiGate bootstrap, user_data, default config, HA configuration

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Default firewall configuration
{: #default-firewall-configuration}

Each FortiGate firewall offering is deployed with a default bootstrap configuration that is applied at provisioning time through the `user_data.conf` file. This configuration sets the hostname and defines the default interface aliases and access settings. 
{: shortdesc}

[Chida]{: tag-purple} The Single VM configuration differs from the HA configurations. Need to describe these separately. Rabindra worked on the various features. Please work with him to document why you would want one configuration vs the other and list the differences in features.

## Single VM default configuration
{: #single-vm-default-config}

The following default configuration is applied to the Single VM offering at deployment time.

[Chida]{: tag-purple} Andrew showed me this in the UI. I believe this needs to be spelled out.

```text
config system global
set hostname FGT-IBM
end
config system interface
edit port1
set alias untrust
set allowaccess https ssh ping
next
edit port2
set alias trust
set allowaccess https ssh ping
next
end
```
{: codeblock}

The configuration sets the following defaults:

- **Hostname** — `FGT-IBM`
- **port1** — aliased as `untrust` (public-facing interface); allows HTTPS, SSH, and ping access
- **port2** — aliased as `trust` (private interface); allows HTTPS, SSH, and ping access

## HA Single Zone and HA Cross Zone default configuration
{: #ha-default-config}

The HA Single Zone and HA Cross Zone offerings use a different default configuration from the Single VM offering.  
{: note}

[Chida]{: tag-purple} Please let me know if Rabindra can't assist on this topic.  Again, need to have completed by mid July. 

Other feedback that needs to be factored into this documentation.

_CISO was concerned with the fact that "by default" we install a floating IP. So it's internet facing. Customers without a FIP can't get to the firewall. So the compromise that we came to was that (as you know) every network interface has to have a security group. We will be creating a new security group with each firewall deployment._

_The security group for all inbound rules will be "deny all" by default. So we need to give them examples of the outbound rules._

_The outbound rules will be wide open, but the inbound rules will be locked down. There will be 1 rule that allows HA clustering, but otherwise they will not be able to get to the FIP, essentially. So, we need to state this up front, and then we need to say to access your firewalls, floating IP for management. Access the security group and add an inbound rule that allowlists your source IP to have access to SSH or to HTTP._

_Need to be clear.  Add in the ordering section and then have a security best practices topic.  Need to search the classic firewall docs (may be in Juniper vSRX).  "Working with the default config section" - It's under Adv Tasks -- securiting the host operating system. FW security best practices._

_This point is important because customers, they'll order a licensed firewall, they'll see the FIP, and then they'll try to access it and won't be able to._
