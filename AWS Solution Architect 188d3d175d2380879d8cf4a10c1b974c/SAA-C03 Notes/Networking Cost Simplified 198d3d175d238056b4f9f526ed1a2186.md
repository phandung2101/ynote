# Networking Cost Simplified

# Cost in AWS per GB

- Free traffic in
- Same AZ - free if using private IP
- Cross AZ - $0.01 if using private IP
- Cross AZ - $0.02 if using public IP/elastic IP
- Cross Region - $0.02 Inter-region

⇒ recommend using private IP for good saving and better network performance

# Minimizing ergress traffic network cost

- Egress traffic: outbound traffic (from AWS to outside)
- Ingress traffic: inbound traffic - from outside to AWS (typically free)
- Try to keep as much internet traffic within AWS to minimize cost

# S3 Data Transfer Pricing - Analysis for USA

- S3 ingress: free
- S3 to Internet: $0.09 per GB
- S3 Transfer Acceleration:
    - Faster transfer time (50-500% better)
    - Addtional cost on top of Data Transfer Pricing: +$0.04 to $0.08 per GB
- S3 to CloudFront: $0.00 per GB
- CloudFront to Internet: $0.085 per GB (slightly cheaper than S3)
    - Caching capability (lower latency)
    - Reduce costs associated with S3 Request Pricing (7x cheaper with CloudFront)
- S3 Cross Region Replication: $0.02 per GB

⇒ D: Seem like using CF with S3 to serve static content type is best solution in here.

# Pricing NAT Gateway vs Gateway VPC Endpoint

- Bad solution: Private EC2 → Public NAT Gateway → Internet Gateway → Internet → S3 Bucket. This will cost you:
    - $0.045 NAT Gateway / hour
    - $0.045 NAT Gateway data processed / GB
    - $0.09 Data transfer out to S3 (cross-region)
    - $0.00 Data transfer out to S3 ( same-region)
- Good solution: Private EC2 → VPC Endpoint → S3. This cost you only:
    - No cost for using Gateway Endpoint
    - $0.01 Data transfer in/out (same-region)