---

copyright:
  years: 2026
lastupdated: "2026-07-15"

keywords: access fortigate, FortiGate web console, SSH fortigate, floating IP, fortigate login, security group inbound rule

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Accessing your FortiGate firewall
{: #access-firewall}

After you deploy a FortiGate firewall, you can access it through the FortiGate web console or SSH.
{: shortdesc}

## Before you begin
{: #access-firewall-prereqs}

- The FortiGate deployment must be complete and in a running state.
- You need the public floating IP address and initial administrator password from the Schematics workspace output. The output variable names differ by topology.
- The initial administrator password might be empty on the first start. If the password field is empty, use the virtual server instance ID as the initial password.

| Topology | Public IP output | Password output |
|---|---|---|
| Single VM | `FortiGate_Public_IP` | `Default_Admin_Password` |
| HA Single Zone | `FortiGate_Public_IP` (active node `port1`) | `FGT1_Default_Admin_Password`, `FGT2_Default_Admin_Password` |
| HA Cross Zone | `FGT1_Port1_Public_IP`, `FGT2_Port1_Public_IP` | `FGT1_Default_Admin_Password`, `FGT2_Default_Admin_Password` |
{: caption="Schematics workspace output variables by topology" caption-side="bottom"}

The firewall is not reachable immediately after deployment. A dedicated security group is created automatically and denies all inbound traffic by default. You will see a floating IP in your VPC resources, but connection attempts will time out until you complete Step 1.
{: note}

## Step 1: Allow management access in the security group
{: #access-firewall-security-group}

The security group that is created during deployment denies all inbound traffic by default. Add an inbound rule to allow access from your IP address before you can connect.

1. In the [IBM Cloud console](/login), click the navigation menu and select **VPC Infrastructure > Security groups**.
1. Locate the security group that was created for your FortiGate deployment. It is named after your deployment cluster.
1. Click the security group name to open it.
1. On the **Rules** tab, click **Create**.
1. Set **Direction** to **Inbound**.
1. Set **Protocol** to **TCP**.
1. Set the **Port range** to **443** for HTTPS access or **22** for SSH access.
1. In **Source type**, select **IP address** and enter your administrator IP address or CIDR range.
1. Click **Create** to save the rule.

Restrict inbound access to known administrator IP addresses only. Avoid using `0.0.0.0/0` as the source.
{: important}

For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups).

## Step 2: Choose your management access method
{: #access-firewall-access-method}

Two methods are available to access the FortiGate web console. Use the method that best fits your security requirements.

### Method 1: Floating IP with allowlist (default)
{: #access-method-fip}

The floating IP on `port1` is assigned automatically and is internet-routable. To use it for management access, add an inbound security group rule (see step 1) that restricts access to your administrator IP address.

### Method 2: VPN access (no floating IP required)
{: #access-method-vpn}

Configure a VPN connection into your VPC and access the FortiGate web console by using its private IP address on port 443. This method eliminates direct internet-facing management access entirely and does not require modification of the floating IP configuration. For guidance on setting up VPN access, see [Use a VPN or bastion host for management access](/docs/licensed-firewall?topic=licensed-firewall-fortigate-security-best-practices#bp-vpn-management).

## Step 3: Route traffic through the firewall
{: #access-firewall-routing}

The floating IP on `port1` makes the firewall reachable from the internet for management purposes. However, for the FortiGate to inspect and control traffic between your VPC subnets or between your VPC and the internet, you must configure VPC routing to send traffic through the firewall.

To route traffic through the FortiGate, update the VPC routing tables so that the FortiGate's private interface (`port2`) is the next hop for the traffic you want to inspect:

1. In the [IBM Cloud console](/login), click the navigation menu and select **VPC Infrastructure > Network > Routing tables**.
1. Select the routing table associated with the subnet whose traffic you want to route through the firewall.
1. Click **Create route**.
1. Set the **Destination CIDR** to the traffic that you want to inspect (for example, `0.0.0.0/0` for all outbound internet traffic, or a specific subnet CIDR for inter-subnet traffic).
1. Set the **Next hop** type to **IP address** and enter the private IP address of the FortiGate `port2` interface.
1. Click **Save**.

Repeat this for each subnet whose traffic needs to flow through the firewall.

For traffic to flow correctly, the FortiGate must also have a firewall policy that allows the traffic between the source and destination interfaces (`port1` and `port2`). Without a matching allow policy, the FortiGate drops the traffic even if routing is correctly configured. For guidance on creating firewall policies, see the [FortiGate Administration Guide](https://docs.fortinet.com/product/fortigate/8.0){: external}.
{: important}

## Step 4: Log in to the FortiGate web console
{: #access-firewall-login}

Use the public IP address from the Schematics workspace output for your topology (see [Before you begin](#access-firewall-prereqs)). For HA deployments, you can log in to either node using its individual `port4` HA management IP, or to the active node that uses the `port1` public IP.

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

SSH access requires a TCP port 22 inbound rule in the security group in addition to any HTTPS rule.
{: note}

## Next steps
{: #access-firewall-next-steps}

- [Configure firewall policies](https://docs.fortinet.com/product/fortigate/8.0){: external} using the FortiGate Administration Guide on the Fortinet documentation site.
- [Enable security services](/docs/licensed-firewall?topic=licensed-firewall-enable-security-services) to activate IPS, antivirus, and web filtering.
