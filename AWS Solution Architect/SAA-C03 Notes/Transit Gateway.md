# Transit Gateway

Network topology could become very complicated. So we going easier solution **Transit Gateway**

# Transit Gateway

- **For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection**
- Regional resource, can work cross-region
- Share cross-account using Resource Access Manager (RAM)
- You can peer Transit Gateways across regions
- Route Tables: limit which VPC can talk with other VPC
- Works with Direct Connect Gateway, VPN connections
- Supports **IP Multicast** (not supported by any other AWS service)

# Site-to-Site VPN ECMP

- **ECMP = Equal-cost multi-path routing**
- Routing strategy to allow forward a packet over multiple best path
- Use case: create multiple Site-to-Site VPN connections **to increase the bandwitdth of your connection to AWS**

Scenario: Usually using Site-to-Site VPN connect dirrectly VPN we can’t increase bandwitdth ⇒ so using 1 Site-to-Site VPN we will have 2 tunnel ⇒ using multiple Site-to-Site VPN connect to Transit Gateway give us (2n) tunnels ⇒ more bandwidth

# Throughput with ECMP

![image.png](AWS%20Solution%20Architect/SAA-C03%20Notes/Transit%20Gateway%20192d3d175d238076b236f3c3871ed88b/image.png)

# Share Direct Connect between multiple accounts

![image.png](image%201.png)