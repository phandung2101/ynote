# IPv6 in VPC

- **IPv4 cannot disable for your VPC and subnet**
- You can enable IPv6  (they’re public IP addresses) to operate in dual-stack mode
- Your EC2 get at least
    - 1 private IPv4
    - 1 public IPv6
- They can communicate using either IPv6 or IPv4 to the internet through Internet Gateway

# IPv6 Troubleshooting

- So if you cannot launch an EC2 in your subnet:
    - No IPv4 is available in subnet
    - IPv6 space very large so not because of IPv6
- Solution is create new IPv4 CIDR with larger space in subnet