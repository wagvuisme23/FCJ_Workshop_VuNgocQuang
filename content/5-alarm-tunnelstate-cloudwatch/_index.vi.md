---
title : "Tạo Alarm giám sát TunnelState trong AWS CloudWatch"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5. </b> "
---

### Tạo Alarm giám sát TunnelState trong AWS CloudWatch

Mục tiêu của bước này là cấu hình giám sát trạng thái của từng VPN Tunnel bằng Amazon CloudWatch. Khi một tunnel gặp sự cố (TunnelState = 0), hệ thống sẽ tự động gửi cảnh báo để kỹ thuật viên có thể thực hiện failover hoặc khắc phục ngay.

#### Các bước thực hiện trên AWS Management Console

1. **Truy cập CloudWatch**
   - Vào dịch vụ **CloudWatch** trên AWS Management Console.
   - Trong menu bên trái, chọn **Metrics** → **All metrics**.

![Alarm](/images/5/0001.png?featherlight=false&width=90pc)

2. **Chọn metric TunnelState**
   - Chọn namespace: **AWS/VPN**.
   - Chọn chế độ lọc **By Tunnel**.
   - Tìm metric **TunnelState** cho tunnel bạn muốn giám sát (ví dụ Tunnel 1).
   - Tick chọn metric này.

3. **Tạo alarm**
   - Nhấn **Actions** (trên cùng bên phải) → **Create alarm**.

![Alarm](/images/5/0002.png?featherlight=false&width=90pc)

4. **Cấu hình điều kiện alarm**
   - **Metric name**: `TunnelState`
   - **Statistic**: `Minimum` (đảm bảo nếu trong chu kỳ có thời điểm TunnelState = 0 thì vẫn kích hoạt alarm).
   - **Period**: `1 minute`
   - **Threshold type**: Static
   - **Whenever TunnelState is**: Chọn `Lower/Equal` → nhập giá trị `0`

![Alarm](/images/5/0003.png?featherlight=false&width=90pc)

5. **Cấu hình hành động (Action)**
   - **Alarm state trigger**: `In alarm`
   - **Send notification to**: Chọn SNS topic hiện có hoặc tạo mới.
   - Nếu tạo mới:
     - Chọn **Create new topic**
     - Đặt tên topic (ví dụ: `vpn-alerts`)
     - Nhập địa chỉ email nhận thông báo
     - AWS sẽ gửi email xác nhận → vào email và click xác nhận để kích hoạt.

![Alarm](/images/5/0004.png?featherlight=false&width=90pc)

![Alarm](/images/5/0005.png?featherlight=false&width=90pc)

6. **Đặt tên và tạo alarm**
   - **Alarm name**: ví dụ `VPN-Tunnel1-Down`
   - **Description**: “Cảnh báo khi VPN Tunnel 1 mất kết nối (TunnelState=0)”
   - Nhấn **Create alarm** để hoàn tất.

![Alarm](/images/5/0006.png?featherlight=false&width=90pc)

![Alarm](/images/5/0007.png?featherlight=false&width=90pc)

7. **Lặp lại cho Tunnel 2**
   - Quay lại **All metrics → AWS/VPN → By Tunnel** và tìm metric **TunnelState** của Tunnel 2.
   - Tạo alarm tương tự để đảm bảo cả 2 tunnel đều được giám sát.

#### Kết quả mong đợi
- Khi một tunnel bị mất kết nối, giá trị `TunnelState` sẽ chuyển sang `0`.
- CloudWatch Alarm sẽ kích hoạt và gửi email cảnh báo đến nhóm kỹ thuật.
- Nhóm kỹ thuật có thể theo dõi quá trình failover sang tunnel dự phòng và thực hiện các biện pháp khắc phục.

---
