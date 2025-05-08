# Security Groups & NACLs

# Network Access Control List

- NACL are like a firewall which control traffic from and to subnets
- **One NACL per subnet,** new subnets are assigned to **Default NACL**
- You can define **NACL Rules:**
    - number (1-32766), higher precedence with a lower number
    - first rule match will drive the decision
    - example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP address will be allowed because 100 has a higher precedence over 200
    - The last rule is an asterisk (*) and denies a request in case of no rule match
    - AWS recommends add rules by increment of 100
- Newly created NACLs will deny everything
- NACL are a greate way of blocking a specific IP address at the subnet level

# Default NACL

- accepts everything inbound/outbound with the subnets it’s associated with
- **Do not modify** default NACL, instead create custom NACLs

# Ephemeral Ports

- two endpoints to establish a connection, they must use ports
- clients connect to a **defined port**, and expect a response on an **ephemeral port**
- different os use different port ranges, ex:
    - IANA & MS Window 10 → 49152 - 65535
    - Many Linux Kernels → 32768 - 60999

# NACL with Ephemeral Ports

![image.png](AWS%20Solution%20Architect%20188d3d175d2380879d8cf4a10c1b974c/SAA-C03%20Notes/Security%20Groups%20&%20NACLs%20191d3d175d2380149cdac4400f7b9924/image.png)