# VPC Basic Knowledge

# CIDR

Part xx of this `aaa.bbb.ccc.ddd/xx` present for number of accessible IP

# Default VPC

- all new aws acc have a default VPC
- have inbound/outbound rule of VPC. Default is accept all

# VPC

- stand for Virtual Private Cloud
- Max 5 VPC per Region ( soft limit )
- Max CIDR per VPC is 5, each CIDR
    - Min size is /28 (16 addr)
    - Max size is /16  (65536 addr)
- VPC is private, only private IPv4 ranges are allowed

# Subnet

- AWS reserver **5 IP addresses (first 4 & last 1)** in each subnet
    - Could not be use and assigned to an EC2 instance ****
    - 10.0.0.0 - Network address
    - 10.0.0.1 - VPC router
    - 10.0.0.2 - mapping to Amazon-provided DNS
    - 10.0.0.3 - AWS future use
    - 10.0.0.255 - Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved
- **Exam Tip:** if you need 29 IP addresses for EC2 instances:
    - You can’t choose a subnet of size /27 (32 IP address, 32-5=27<29)
    - You need to choose a subnet of size /26 (64 IP address, 64-5=59>29)

# Internet Gateways (IGW)

- Allow resources in VPC connect to the Internet
- It scales horizontally and HA and redundant
- Create separately from a VPC
- One VPC can only attached to one IGW and vice versa
- IGW on thier own do not allow Internet access → Route tables must also be edited!

# Route table

- route traffic in subnet. 1 route table contains multi subnet.
- Rule to route traffic. Example:
    - 10.0.0.0/16 - local network VPC
    - 0.0.0.0/0 - IGW (we mention before to connect to internet)