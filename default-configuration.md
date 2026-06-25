---

copyright:
  years: 2026

lastupdated: "2026-06-25"

keywords: firewall default configuration, FortiGate bootstrap, user_data, default config, HA configuration

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Default firewall configuration
{: #default-firewall-configuration}

Each FortiGate firewall offering is deployed with a default bootstrap configuration that is applied at provisioning time through the `user_data.conf` file. This configuration sets the hostname and defines the default interface aliases and access settings. The Single VM configuration differs from the HA configurations.
{: shortdesc}

## Single VM default configuration
{: #single-vm-default-config}

The following default configuration is applied to the Single VM offering at deployment time.

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

The HA Single Zone and HA Cross Zone offerings use a different default configuration from the Single VM offering. For details on the HA default configuration, contact Rabindra.
{: note}


Other feedback:

_CISO was concerned with the fact that by default, we install a floating IP. So it's internet facing.

They didn't like that. But customers without a FIP can't get to the firewall. So the compromise that we came to was, you know, every network interface has to have a security group. And we, this is the important part, we will be creating a brand new security group with each firewall deployment.

And the security group for all inbound rules will be "deny all" by default. So we need to give them examples of the outbound rules.

The outbound rules will be wide open, but the inbound rules will be locked down. There will be 1 rule in there that allows HA clustering, but otherwise they will not be able to get to the FIP, essentially. So what we need to state, we need to state that up front, and then we need to say,

you know, to access your firewalls, floating IP for management. Access the security group and add an inbound rule that allowlists your source IP to have access, you know, to SSH or to HTTP._

Need to be clear.  In ordering section and then have security best practices.  Might be in classic docs. Dana needs to look... maybe in Juniper vSRX.  Working with the default config section.

It's under Adv Tasks -- securiting the host operating system. FW security best practices._

So that point is important because customers, they'll order this thing, they'll see the FIP and they'll try to access it and they'll be like, what the heck?
