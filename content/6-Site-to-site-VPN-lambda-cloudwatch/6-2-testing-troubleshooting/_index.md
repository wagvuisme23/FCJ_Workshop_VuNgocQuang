---
title : "Testing & Troubleshooting"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---

## Testing

1. **Test via EventBridge** (recommended):

   - EventBridge → Rules → select the rule `alarm-vpn-tunnel1-to-lambda` → **Test** → **Create test event** → paste the sample event JSON (provided in the workshop documentation) → Send test event.
   - Check Lambda → Monitoring → View logs in CloudWatch.

2. **Test by invoking Lambda directly**:

   - Lambda → select the function → **Test** → paste `{}` or a sample event → Run.

3. **End-to-end (real scenario)**:

   - Trigger a real alarm (e.g., block UDP 500/4500 outbound to AWS\_T1\_OUT so that TunnelState = 0) → when the Alarm state changes to ALARM, EventBridge will invoke Lambda → Lambda calls SSM → check the SSM command output.

---

## Troubleshooting quick guide

Summary table: symptom → possible cause → quick check/command (console & on-prem). Detailed step-by-step instructions follow the table.

| Symptom                      |                                                Possible causes | Quick fixes / commands                                                                                                                 |
| ---------------------------- | -------------------------------------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------- |
| Tunnel DOWN (AWS)            | PSK mismatch, outside IP blocked, NAT-T blocked (UDP 4500/500) | Check AWS Tunnel details; on-prem: `show crypto isakmp sa` / `show crypto ipsec sa` or `sudo ipsec statusall`                          |
| BGP not established          |     Wrong inside IP, ASN mismatch, TCP 179 blocked, BGP timers | Verify inside IPs; verify ASN; `show ip bgp summary` on the router; check security groups/NACL (BGP runs over inside IPs through VPN) |
| Intermittent packet loss     |            MTU/fragmentation, packet drop on path, IPSec rekey | Check IPSec SA counters; reduce MTU; enable DF handling; run continuous ping/iperf                                                     |
| Routes not propagated to VPC |                Route propagation disabled, BGP not advertising | VPC → Route Tables → ensure route propagation is enabled for VGW; AWS CLI: `aws ec2 describe-route-tables`                            |

---

## Common troubleshooting scenarios

**EventBridge does not invoke Lambda**: Check the Lambda function’s resource-based policy (Permissions → Resource-based policy). If needed, add it using the CLI:

```bash
aws lambda add-permission \
  --function-name vpn-diagnostic-handler \
  --statement-id eventbridge-invoke \
  --action "lambda:InvokeFunction" \
  --principal events.amazonaws.com \
  --source-arn arn:aws:events:<region>:<account-id>:rule/alarm-vpn-tunnel1-to-lambda
```

**SSM does not execute the command**: Check that the EC2 instance has the SSM Agent installed and the correct Instance Profile attached.

---