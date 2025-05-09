# Blocking an IP address

# Need blocking basic

- using NACL for the need block some specify at VPC level
- using SG with public ec2 ip
- Optional Firewall software in EC2 (consume EC2 cpu → not really effective)

# Blocking an IP address - with an ALB

Client → ALB → EC2 private IP

- using ALB to route traffic to EC2
- ALB connection termination so client not directly connect to EC2
- Use NACL to block at VPC level if needed

# Blocking an IP address  -  ALB, CloudFront WAF

Client → CloudFront Public IPs → ALB → EC2 private IP

In this case ALB take traffic from CloudFront Public IP ( this resource is outside of VPC ) so we cannot use NACL because traffic to ALB is from CF (we do not know client IP)

- Need config WAF IP address filtering
- Could use CloudFront Geo Restriction for add more secure layer