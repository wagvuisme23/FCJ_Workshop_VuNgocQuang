---
title : "Testing & Troubleshooting"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---

## Testing / Kiểm thử

1. **Test qua EventBridge** (khuyến nghị):

   - EventBridge → Rules → chọn rule `alarm-vpn-tunnel1-to-lambda` → **Test** → **Create test event** → paste sample event JSON (mẫu có trong tài liệu workshop) → Send test event.
   - Kiểm tra Lambda → Monitoring → View logs in CloudWatch.

2. **Test invoke trực tiếp Lambda**:

   - Lambda → chọn function → **Test** → paste `{}` hoặc sample event → Run.

3. **End-to-end (thực tế)**:

   - Kích hoạt alarm thật (ví dụ chặn UDP 500/4500 outbound tới AWS\_T1\_OUT để TunnelState = 0) → khi Alarm chuyển ALARM, EventBridge sẽ invoke Lambda → Lambda gọi SSM → check SSM command output.

---

## Troubleshooting quick guide (Hướng dẫn xử lý nhanh)

Bảng tóm tắt symptom → nguyên nhân có thể → lệnh/kiểm tra nhanh (console & on‑prem). Sau bảng là hướng dẫn chi tiết từng bước.

| Symptom                      |                                                Possible causes | Quick fixes / commands                                                                                                                 |
| ---------------------------- | -------------------------------------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------- |
| Tunnel DOWN (AWS)            | PSK mismatch, outside IP blocked, NAT-T blocked (UDP 4500/500) | Check AWS Tunnel details; on‑prem: `show crypto isakmp sa` / `show crypto ipsec sa` or `sudo ipsec statusall`                          |
| BGP not established          |     Wrong inside IP, ASN mismatch, TCP 179 blocked, BGP timers | Verify inside IPs; verify ASN; `show ip bgp summary` on router; check security groups/NACL (though BGP is over inside IPs through VPN) |
| Intermittent packet loss     |            MTU/fragmentation, packet drop on path, IPSec rekey | Check IPSec SA counters; reduce MTU; enable DF handling; run continuous ping/iperf                                                     |
| Routes not propagated to VPC |                Route propagation disabled, BGP not advertising | VPC → Route Tables → Route propagation enabled for VGW; AWS: `aws ec2 describe-route-tables`                                           |

---

## Troubleshooting (thường gặp)

**EventBridge không invoke Lambda**: kiểm tra resource-based policy của Lambda (Permissions → Resource-based policy). Nếu cần, thêm bằng CLI:

```bash
aws lambda add-permission \
  --function-name vpn-diagnostic-handler \
  --statement-id eventbridge-invoke \
  --action "lambda:InvokeFunction" \
  --principal events.amazonaws.com \
  --source-arn arn:aws:events:<region>:<account-id>:rule/alarm-vpn-tunnel1-to-lambda
```

**SSM không thực thi lệnh**: kiểm tra EC2 có SSM Agent & Instance Profile

---

