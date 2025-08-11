---
title : "Kiểm tra ngay trên AWS Management Console"
displayDate :  "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 6.5 </b> "
---

### Kiểm tra ngay trên AWS Management Console

#### Kiểm tra trạng thái Tunnel & BGP

1. VPC Console → **Site-to-Site VPN Connections** → chọn VPN connection.
2. Mở tab **Tunnel Details**:

   - Kiểm tra **Status** (UP/DOWN) cho Tunnel 1 & Tunnel 2.
   - Kiểm tra **BGP Status** (Established / Idle).
   - Sao chép **Outside IPs** và **Inside IPs** để so sánh với config on‑prem.

#### Kiểm tra CloudWatch metrics

1. CloudWatch → Metrics → All metrics → `AWS/VPN` → By Tunnel.
2. Xem metric **TunnelState** để xác định thời điểm down.
3. Xem **TunnelDataIn/Out** để phát hiện spike hoặc chuyển hướng lưu lượng.

#### Kiểm tra Route propagation

1. VPC → Route Tables → chọn route table của subnet cần reach on‑prem.
2. Tab **Route propagation** → đảm bảo **VGW** được bật.
3. Tab **Routes** → xác nhận route cho on‑prem CIDR trỏ tới `vgw-xxxx`.

---

### Kiểm tra trên thiết bị On‑Premises (một vài lệnh tham khảo)

> Các lệnh bên dưới mang tính tham khảo; tuỳ vendor (Cisco/Juniper/VyOS/pfSense) cú pháp khác nhau.

#### Kiểm tra ISAKMP / IKE SA

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

#### Kiểm tra BGP

- **Cisco**:

```text
show ip bgp summary
show ip bgp neighbors
```

- **VyOS**:

```bash
show ip bgp summary
```

#### Kiểm tra firewall / NAT

- Kiểm tra firewall rule có cho phép UDP 500/4500, và không NAT outside IP (hoặc NAT đúng)
- Dùng tcpdump/wireshark để bắt gói trên interface public: `sudo tcpdump -n -i eth0 port 500 or port 4500`

#### Kiểm tra MTU / Fragmentation

- Trên on‑prem, thử ping với DF flag và giảm MTU:

```bash
ping -M do -s 1400 <ONPREM_PEER_IP>
```

- Nếu ping thành công với size nhỏ nhưng thất bại với size lớn, cân nhắc giảm MTU hoặc enable MSS clamping trên firewall.

---

{{% notice note %}}
⚠️ **Lưu ý cuối cùng**

- Trong mọi test, hãy thực hiện trong maintenance window hoặc thông báo tới teams liên quan.
- Không thay đổi cả hai tunnel cùng lúc khi test failover.
- Lưu secrets (PSK, webhook) trong **AWS Secrets Manager** thay vì đặt trực tiếp trong Lambda env vars.
{{% /notice %}}

---