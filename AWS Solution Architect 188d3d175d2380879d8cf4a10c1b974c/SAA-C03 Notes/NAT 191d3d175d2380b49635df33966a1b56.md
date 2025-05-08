# NAT

# NAT

- Network Address Translation

# NAT Instance (Outdated, but still at the exam)

- allow EC2 in private subnets to connect to the Internet
- Must be launched in a public subnet
- Must disable EC2 setting: Source/Destionation check
- Must have Elastic IP attached to it
- Route tables must config to route traffic from private subnet to the NAT instance

# NAT Gateway

- AWS-managed NAT, higher bandwidth. HA, no admin
- Pay per hour for usage and bandwitdth
- NATGW is created in a specific AZ, uses an Elastic IP
- Can’t be used by EC2 instance in the same subnet ( only other subnets)
- Requires an IGW (Private Subnet ⇒ NATGW ⇒ IGW)
- 5 Gbps of bandwidth with automatic scaling up to 100 Gbps
- No Security Groups to manage / required

# NAT Gateway with High Availability

- NAT Gateway is resilient within single AZ
- Must create multiple NAT Gateways in multiple AZs for fault-tolerance
- There is no cross-AZ failover needed because if an AZ goes down it doesn’t need NAT