# Firewall Manager

- **Manage rules in all accounts of an AWS Organization**
- Security policy: common set of security rules
    - WAF rules
    - AWS Shield Advanced
    - SG for EC2, ALB and ENI resources in VPC
    - AWS Network Firewall (VPC Level)
    - Route 53 Resolver DNS Firewall
    - Policies are created at the region level
- **Rules are applied to new resources as they are created ( good for compliance) across all and future accounts in Organization**

# WAF vs. Firewall Manager vs. Shield

- **WAF, Firewall Manager, Shield can used together to comprehensive protection**
- Define your web ACL rules in WAF
- For granular protections of resources → WAF Alone is correct choice
- AWS WAF across acc → accelerate WAF configuration, auto protect new resource → use Firewall Manager with AWS WAF
- Shield Advanced add additional features on top of AWS WAF, such as dedicated support from the SRT and advanced reporting
- If prone to frequent DDoS attacks, consider purchasing Shield Advanced