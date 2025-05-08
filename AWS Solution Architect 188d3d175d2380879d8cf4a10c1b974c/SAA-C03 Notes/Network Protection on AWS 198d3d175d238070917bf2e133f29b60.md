# Network Protection on AWS

- To protect network on AWS, we’ve seen
    - Network Access Control Lists (NACLs)
    - Amazon VPC security groups
    - AWS WAF (protect against milicious request)
    - AWS Shield & AWS Shield Advanced
    - AWS Firewall Manager ( to manage them across accounts)
- But what if we want to protect in a sophisticated way our entire VPC?

# AWS Network Firewall

- protect entire Amazon VPC
- From L3 → L7 protection
- Any direction, you can inspect
    - VPC to VPC traffic
    - Outbound to internet
    - Inbound to internet
    - To/from DX and Site-to-Site VPN
- Internally, the AWS Network Firewall uses the AWS Gateway Load Balancer
- Rules can be centrally managed cross-account by AWS Firewall Manager to apply to many VPCs

# Network Firewall - Fine Grained Controls

- supports 1000s of rules
    - ip & port - example: 10000s of IPs filtering
    - Protocol - example: block the SMB protocol for outbound communications
    - Stateful domain list rule groups: only allow outbound traffic to *.mycorp.com or third-party software repo
    - General pattern matching using regex
- Traffic filtering: Allow, drop, or alert for the traffic that matches the rules
- Active flow inspection to protect against network threats with intrusion-prevention capabilities (like Gateway Load Balancer, but all managed by AWS)
- Send logs of rule matches to S3, CW Logs, KDF