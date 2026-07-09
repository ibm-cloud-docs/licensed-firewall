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

## Before you begin
{: #access-firewall-prereqs}

- The FortiGate deployment must be complete and in a running state.
- You need the public floating IP address assigned to port1 of your FortiGate instance. This is displayed as `FortiGate_Public_IP` in the Schematics workspace output after deployment.
- You need the initial administrator password, which is displayed as `Default_Admin_Password` in the Schematics workspace output after your order completes successfully.

## Default network security posture
{: #access-firewall-default-posture}

Understanding the default network configuration helps explain why the firewall is not reachable immediately after deployment.

When deployment completes, the following is true by default:

- A floating IP is assigned to port1 (the public-facing interface). This IP is internet-routable and visible in your VPC.
- A dedicated security group is created and attached to all FortiGate network interfaces.
- All inbound traffic is denied by default. No traffic can reach the firewall from the internet or your VPC until you explicitly allow it.
- All outbound traffic is allowed by default. The firewall can initiate outbound connections, which is required for license activation and FortiGuard updates.
- For HA deployments, one inbound rule is pre-configured to allow HA heartbeat traffic between the two FortiGate nodes on the cluster sync interface.

This default posture ensures that your firewall is not openly reachable on the internet immediately after provisioning. You will see a floating IP in your VPC resources, but attempts to connect to it in a browser or over SSH will time out until you add an inbound security group rule that allows access from your administrator IP address.
{: important}

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

## Step 2: Route traffic through the firewall
{: #access-firewall-routing}

The floating IP on port1 makes the firewall reachable from the internet for management purposes. However, for the FortiGate to actually inspect and control traffic between your VPC subnets or between your VPC and the internet, you must configure VPC routing to send traffic through the firewall.

To route traffic through the FortiGate, update the VPC routing tables so that the FortiGate's private interface (port2) is the next hop for the traffic you want to inspect:

1. In the [IBM Cloud console](https://cloud.ibm.com){: external}, click the navigation menu and select **VPC Infrastructure > Network > Routing tables**.
1. Select the routing table associated with the subnet whose traffic you want to route through the firewall.
1. Click **Create route**.
1. Set the **Destination CIDR** to the traffic you want to inspect (for example, `0.0.0.0/0` for all outbound internet traffic, or a specific subnet CIDR for inter-subnet traffic).
1. Set the **Next hop** type to **IP address** and enter the private IP address of the FortiGate port2 interface.
1. Click **Save**.

Repeat this for each subnet whose traffic should flow through the firewall.

For traffic to flow correctly, the FortiGate must also have a firewall policy that allows the traffic between the source and destination interfaces (port1 and port2). Without a matching allow policy, the FortiGate drops the traffic even if routing is correctly configured. For guidance on creating firewall policies, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.
{: important}

## Step 3: Log in to the FortiGate web console
{: #access-firewall-login}

1. Open a web browser and navigate to `https://<FortiGate_Public_IP>`.
1. Accept the self-signed certificate warning if prompted.
1. Log in with the username `admin` and the initial password from the Schematics workspace output.
1. When prompted, change the administrator password to a strong, unique value.

## Step 4: Connect by using SSH (optional)
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

- [Configure firewall policies](https://docs.fortinet.com/product/fortigate/8.0){: external} using the FortiGate Administration Guide on the Fortinet documentation site.
- [Enable security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services) to activate IPS, antivirus, and web filtering.
