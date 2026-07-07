---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: access fortigate, FortiGate web console, SSH fortigate, floating IP, fortigate login, security group inbound rule

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Accessing the FortiGate web console
{: #access-firewall}

After you deploy a FortiGate firewall, you can access the management interface through a web browser or SSH. Before you can connect, you must add an inbound rule to the security group that was created for the firewall.
{: shortdesc}

## Default network security posture
{: #access-firewall-default-posture}

Understanding the default network configuration helps explain why the firewall is not reachable immediately after deployment.

When deployment completes, the following is true by default:

- A **floating IP** is assigned to port1 (the public-facing interface). This IP is internet-routable and visible in your VPC.
- A **dedicated security group** is created and attached to all FortiGate network interfaces.
- All **inbound rules are deny-all** — no traffic can reach the firewall from the internet or your VPC until you explicitly allow it.
- All **outbound rules are open** — the firewall can initiate traffic outbound, which is required for license activation and FortiGuard updates.
- For HA deployments, **one inbound rule** is pre-configured to allow HA heartbeat traffic between the two FortiGate nodes on the cluster sync interface.

This posture is intentional. The CISO requirement is that the firewall must not be openly reachable on the internet immediately after provisioning. You must explicitly allow your own administrator IP address before you can log in.

This means you will see a floating IP in your VPC resources, but attempts to connect to it in a browser or over SSH will time out until you complete Step 1 below.
{: important}

## Before you begin
{: #access-firewall-prereqs}

- The FortiGate deployment must be complete and in a running state.
- You need the public floating IP address assigned to port1 of your FortiGate instance. This is displayed as `FortiGate_Public_IP` in the Schematics workspace output after deployment.
- You need the initial administrator password, displayed as `Default_Admin_Password` in the Schematics workspace output.

## Step 1: Allow management access in the security group
{: #access-firewall-security-group}

The security group created during deployment denies all inbound traffic by default. You must add an inbound rule to allow access from your IP address before you can connect.

1. In the [IBM Cloud console](https://cloud.ibm.com){: external}, click the navigation menu and select **VPC Infrastructure > Security groups**.
1. Locate the security group that was created for your FortiGate deployment. It is named after your deployment cluster.
1. Click the security group name to open it.
1. On the **Rules** tab, click **Create**.
1. Set **Direction** to **Inbound**.
1. Set **Protocol** to **TCP**.
1. Set the **Port range** to **443** for HTTPS access or **22** for SSH access.
1. Under **Source type**, select **IP address** and enter your administrator IP address or CIDR range.
1. Click **Create** to save the rule.

Restrict inbound access to known administrator IP addresses only. Avoid using `0.0.0.0/0` as the source.
{: important}

## Step 2: Log in to the FortiGate web console
{: #access-firewall-login}

1. Open a web browser and navigate to `https://<FortiGate_Public_IP>`.
1. Accept the self-signed certificate warning if prompted.
1. Log in with the username `admin` and the initial password from the Schematics workspace output.
1. When prompted, change the administrator password to a strong, unique value.

## Step 3: Connect by using SSH (optional)
{: #access-firewall-ssh}

1. Open a terminal on your workstation.
1. Run the following command, replacing `<FortiGate_Public_IP>` with the floating IP address and `<key>` with the path to your SSH private key:

   ```sh
   ssh -i <key> admin@<FortiGate_Public_IP>
   ```
   {: pre}

1. When prompted, enter the administrator password.

SSH access requires a TCP port 22 inbound rule in the security group in addition to any HTTPS rule.
{: note}

## Next steps
{: #access-firewall-next-steps}

- [Configure firewall policies](/docs/licensed-firewall?topic=licensed-firewall-configure-policies) to control traffic through your FortiGate instance.
- [Enable security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services) to activate IPS, antivirus, and web filtering.
