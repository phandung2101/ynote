# Security Layer Best Practice

![image.png](AWS%20Solution%20Architect/SAA-C03%20Notes/Security%20Layer%20Best%20Practice%2018cd3d175d2380ab9a3eec9c064c9f68/image.png)

# AWS Best Practice for DDoS Resiliency

## Common case

- BP1 - CF
    - Web app delivery at the edge
    - protect from DDoS Common Attack
- BP1 - GA
    - access app from the edge
    - integrate with **Shield** for DDoS protection
    - *good when backend not compatible with CloudFront*
- BP3 - Route 53
    - DNS at the edge
    - DDoS protection mecha

## Infra layer defense BP1, BP3, BP6

- protect EC2 against high traffic
- include using GA, Route 53, CF, ELB
- ELB scale with traffic to distribute the traffic to many EC2 instance

## Amazon EC2 with Auto Scaling

- want more protect app to HA → using Auto Scaling when spike traffic could go through all front layer
- **keyword: sudden traffic, flash crowd, DDoS attack**

# App layer defense

- Detect and filter macilous web request (BP1, BP2)
    - CF cache static content and serve it at the edge → protect backend
    - WAF in front of CF and ELB to filter and block request based on request signatures
    - WAF rate-based rule can auto block IPs of bad actor
    - use managed rule on WAF to block attack based on IP reputation or anonymous IPs
    - CF can block specific geographies
- Shield Advanced BP1, BP2, BP6
    - Shield Advanced auto application layer DDoS mitigation auto create, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks

# Attack surface reduction

- Osfuscating AWS resource ( BP1, BP4, BP6 )
    - using CF, API Gateway, ELB to hide your backend → attack never know backend using lambda, ec2, ecs or anything
- Security Group and Network ACLs (BP5)
    - using SG and NACLs to filter traffic based on specific IP at the subnet or ENI-level
    - elastic IP using AWS Shield Advanced
- Protection API endpoint (BP4)
    - hide ec2, lambda, elsewhere
    - edge-optimized mode, or CF + regional mode ( more control for DDoS)
-