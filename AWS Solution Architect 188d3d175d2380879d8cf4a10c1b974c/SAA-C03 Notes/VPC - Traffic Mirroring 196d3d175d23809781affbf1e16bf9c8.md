# VPC - Traffic Mirroring

- Allows you to capture and inspect network traffic in your VPC
- Route the traffic to security appliances that you manage
- Capture the traffic
    - From (source) - ENIs
    - To (targets) - an ENI or a NLB (must be NLB not ALB)
- Capture all packets or capture the packets of your interest (optionally, truncate packets)
- Source and Target can be in the same VPC or different VPCs (VPC Peering)
- Use cases: content inspection, threat monitoring, troubleshooting, …

![image.png](AWS%20Solution%20Architect%20188d3d175d2380879d8cf4a10c1b974c/SAA-C03%20Notes/VPC%20-%20Traffic%20Mirroring%20196d3d175d23809781affbf1e16bf9c8/image.png)