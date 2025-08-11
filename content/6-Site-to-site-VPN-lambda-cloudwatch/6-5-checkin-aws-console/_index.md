---
title : "Verify Directly on AWS Management Console"
displayDate :  "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 6.5 </b> "
---

### Verify Directly on AWS Management Console

#### Check Tunnel & BGP Status

1. VPC Console → **Site-to-Site VPN Connections** → select the VPN connection.
2. Open the **Tunnel Details** tab:

   - Verify **Status** (UP/DOWN) for Tunnel 1 & Tunnel 2.
   - Verify **BGP Status** (Established / Idle).
   - Copy **Outside IPs** and **Inside IPs** to compare with the on-prem configuration.

#### Check CloudWatch Metrics

1. CloudWatch → Metrics → All metrics → `AWS/VPN` → By Tunnel.
2. View the **TunnelState** metric to identify downtime events.
3. View **TunnelDataIn/Out** to detect traffic spikes or rerouting events.

#### Check Route Propagation

1. VPC → Route Tables → select the route table of the subnet that needs to reach on-prem.
2. **Route propagation** tab → ensure **VGW** is enabled.
3. **Routes** tab → verify that the route for the on-prem CIDR points to `vgw-xxxx`.

---

### Verify on On-Premises Device (Sample Commands)

> The commands below are for reference only; syntax may vary depending on the vendor (Cisco/Juniper/VyOS/pfSense).

#### Check ISAKMP / IKE SA

- **Cisco IOS**:

```text
show crypto isakmp sa
show crypto ipsec sa
```

- **VyOS / strongSwan**:

```bash
sudo ipsec statusall
sudo swanctl --list-sas   # nếu dùng swanctl
```

#### Check BGP

- **Cisco**:

```text
show ip bgp summary
show ip bgp neighbors
```

- **VyOS**:

```bash
show ip bgp summary
```

#### Check Firewall / NAT

- Verify that firewall rules allow UDP 500/4500, and that the outside IP is not NATed (or NATed correctly).
- Use tcpdump/wireshark to capture packets on the public interface:  
  `sudo tcpdump -n -i eth0 port 500 or port 4500`

#### Check MTU / Fragmentation

- On the on-prem device, try pinging with the DF flag and reduced MTU:

```bash
ping -M do -s 1400 <ONPREM_PEER_IP>
```

- If ping succeeds with smaller sizes but fails with larger ones, consider lowering MTU or enabling MSS clamping on the firewall.

---

{{% notice note %}}
⚠️ **Final Notes**

- Perform all tests during a maintenance window or notify relevant teams in advance.
- Do not make changes to both tunnels simultaneously when testing failover.
- Store secrets (PSK, webhook) in **AWS Secrets Manager** instead of hardcoding them in Lambda environment variables.
{{% /notice %}}

---
