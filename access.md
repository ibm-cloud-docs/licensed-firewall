---

copyright:
  years: 2026
lastupdated: "2026-10-07"

keywords: access fortigate, FortiGate web console, SSH fortigate, floating IP, fortigate login, security group inbound rule

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Accessing your FortiGate firewall
{: #access-firewall}

After you deploy a FortiGate VM firewall, you can access it through the FortiGate web console or SSH.
{: shortdesc}

The firewall is not reachable immediately after deployment. Two dedicated security groups are created automatically, one for the public interface and one for the private interface. Both include restrictive pre-configured inbound rules that allow the instance to download the license and enable cluster synchronization. They do not permit management access. A floating IP is assigned to `port4` (the management interface) and is visible in your VPC resources, but all connection attempts time out until you complete Step 1.
{: attention}

## Before you begin
{: #access-firewall-prereqs}

- The FortiGate VM deployment must be complete and in a running state.
- You need the management-floating IP address and initial administrator password from the Schematics workspace output. The output variable names differ by topology.
- The initial administrator password might be empty on the first start. If the password field is empty, use the virtual server instance ID as the initial password.

| Topology | Management IP output (`port4`) | Password output |
| --- | --- | --- |
| Single VM | `FortiGate_Public_IP` | `Default_Admin_Password` |
| HA Single Zone | `FGT1_Public_HA_Management_IP`, `FGT2_Public_HA_Management_IP` | `FGT1_Default_Admin_Password`, `FGT2_Default_Admin_Password` |
| HA Cross Zone | `FGT1_Public_HA_Management_IP`, `FGT2_Public_HA_Management_IP` | `FGT1_Default_Admin_Password`, `FGT2_Default_Admin_Password` |
{: caption="Schematics workspace output variables by topology" caption-side="bottom"}

## Step 1: Allow management access in the security group
{: #access-firewall-security-group}

The security groups that are created during deployment deny all inbound management traffic. When you open a security group, you see pre-configured inbound rules for licensing and cluster synchronization. Do not remove these rules. Add a new inbound rule to allow HTTPS or SSH access from your administrator IP address before you can connect.

1. In the [IBM Cloud console](/login), click the navigation menu and select **VPC Infrastructure > Security groups**.
1. Locate the security group for the public interface that was created for your FortiGate VM deployment. It is named after your deployment cluster.
1. Click the security group name to open it.
1. On the **Rules** tab, click **Create**.
1. Set **Direction** to **Inbound**.
1. Set the **Protocol** to **TCP**.
1. Set the **Port range** to **443** for HTTPS access or **22** for SSH access.
1. In **Source type**, select **IP address** and enter your administrator IP address or CIDR range.
1. Click **Create** to save the rule.

Restrict inbound access to known administrator IP addresses only. Avoid using `0.0.0.0/0` as the source.
{: important}

For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups).

## Step 2: Choose your management access method
{: #access-firewall-access-method}

Three methods are available to access the FortiGate web console. Use the method that best fits your security requirements.

### Method 1: Floating IP with allowlist (default)
{: #access-method-fip}

The floating IP on `port4` (management interface) is assigned automatically and is internet-routable. To use it for management access, add an inbound security group rule (see step 1) that restricts access to your administrator IP address.

### Method 2: VPN access (no floating IP required)
{: #access-method-vpn}

Configure a VPN connection into your VPC and access the FortiGate web console by using its private IP address on port `443`. This method eliminates direct internet-facing management access entirely and does not require modification of the floating IP configuration. For guidance on setting up VPN access, see [Use a VPN or bastion host for management access](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices#bp-vpn-management).

### Method 3: VNC or serial console (no security group changes required)
{: #access-method-vnc-serial}

You can access the firewall through the local VNC or serial console that is provided by the virtual server instance. This method is useful for emergency access or initial configuration, and does not require any security group modifications. However, this method is not a permanent solution. For example, it does not work well when you need to upgrade or downgrade firmware.

To open the console, navigate to your virtual server instance in the [IBM Cloud console](/login) under **VPC Infrastructure > Virtual server instances**, click **Actions**, and select **Open VNC console** or **Open serial console**.

## Step 3: Route traffic through the firewall
{: #access-firewall-routing}

The floating IP on `port4` (management interface) makes the firewall reachable for management purposes. `port1` is the public data interface and `port2` is the private data interface. For the FortiGate VM to inspect and control traffic between your VPC subnets or between your VPC and the internet, you must configure VPC routing to send traffic through `port1` and `port2`.

To route traffic through the FortiGate VM, update the VPC routing tables so that the FortiGate VM private interface (`port2`) is the next hop for the traffic you want to inspect:

1. In the [IBM Cloud console](/login), click the navigation menu and select **VPC Infrastructure > Network > Routing tables**.
1. Select the routing table associated with the subnet whose traffic you want to route through the firewall.
1. Click **Create route**.
1. Set the **Destination CIDR** to the traffic that you want to inspect (for example, `0.0.0.0/0` for all outbound internet traffic, or a specific subnet CIDR for inter-subnet traffic).
1. Set the **Next hop** type to **IP address** and enter the private IP address of the FortiGate VM `port2` interface.
1. Click **Save**.

Repeat these steps for each subnet whose traffic needs to flow through the firewall.

For traffic to flow correctly, the FortiGate VM must also have a firewall policy that allows the traffic between the source and destination interfaces (`port1` and `port2`). Without a matching allow policy, the FortiGate VM drops the traffic even if routing is correctly configured. For guidance on creating firewall policies, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.
{: important}

## Step 4: Log in to the FortiGate web console
{: #access-firewall-login}

Use the `port4` management-floating IP address from the Schematics workspace output for your topology (see [Before you begin](#access-firewall-prereqs)). For HA deployments, each node has its own `port4` management IP and you can log in to either node individually.

1. Open a web browser and navigate to `https://<public-ip>`, replacing `<public-ip>` with the appropriate IP address from the workspace output.
1. Accept the self-signed certificate warning if prompted.
1. Log in with the username `admin` and the initial password from the Schematics workspace output.
1. When prompted, change the administrator password to a strong, unique value.

## Step 5: Connect by using SSH (optional)
{: #access-firewall-ssh}

1. Open a terminal on your workstation.
1. Run the following command, replacing `<FortiGate_Public_IP>` with the floating IP address and `<key>` with the path to your SSH private key:

   ```sh
   ssh -i <key> admin@<FortiGate_Public_IP>
   ```
   {: pre}

1. When prompted, enter the administrator password.

SSH access requires a TCP port `22` inbound rule in the security group in addition to any HTTPS rule.
{: note}

## Next steps
{: #access-firewall-next-steps}

- For more information about configuring firewall policies, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.
- [Enable security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services) to activate IPS, antivirus, and web filtering.
