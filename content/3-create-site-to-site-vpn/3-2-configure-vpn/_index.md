---
title : "VPN Connection Configuration"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

#### AWS Site-to-Site VPN Configuration

ℹ️ **Overview**

- This section provides guidance on setting up an AWS Site-to-Site VPN connection.
- Includes the configuration of the Virtual Private Gateway (VGW) and Customer Gateway (CGW).
- Enables secure connectivity between two VPCs through IPSec tunnels.

🔒 **Key Components**

- Virtual Private Gateway (VGW): AWS-side VPN endpoint.
- Customer Gateway (CGW): Represents the customer-side VPN device.
- VPN Connection: IPSec connection between the VGW and CGW.

---

#### Deployment Steps

💡 **Configuration Process**

1. [Create VPG](3-2-1-create-VPG/)
2. [Create Customer Gateway](3-2-2-create-customer-gateway/)
3. [Create VPN Connection](3-2-3-create-vpn-connection/)
