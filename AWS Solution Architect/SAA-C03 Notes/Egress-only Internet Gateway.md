# Egress-only Internet Gateway

- Used for IPv6 only
- (Similar to a NAT Gateway but for IPv6)
- Allow instances in your VPC outbound connections over IPv6 while preventing the internet to initiate an IPv6 connection to your instances
- **You must update the Route Tables**

# Solution compare

1. NAT Gateway (IPv4) → Internet Gateway (IPv4 & IPv6)
2. Egress-only Internet Gateway (IPv6)