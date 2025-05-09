# VPC Flow Logs

- capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Subnet Flow Logs
    - ENI Flow Logs
- help to monitor & troubleshoot connectivity issues
- flow logs data can go to S3, CW Logs, KDF
- capture network information from AWS managed interfaces too: ELB, RDS, ElastiCache, Redshift, WorkSpaces, NATGW, Transit Gateway ….

# VPC Flow Logs Syntax

Just remember some:

- scraddr & dstaddr - help identify problematic IP
- srcport & dstport - help identify problematic ports
- action - success or failure of the request due to SG / NACL
- can be used for analytics on usage patterns, or malicious behavior
- **Query VPC flow logs using Athena on S3 or CW Logs Insights**

# VPC Flow Logs - Troubleshoot SG & NACL issueS

Remember that NACL one way block so if

- Inbound ALLOW and outbound BLOCK
- Outbound ALLOW and inbound BLOCK

⇒ Must be issues because of NACL

# VPC Flow Logs - Architectures

- Flow Logs → CW Logs → CW Contributor Insights → Top-10 IP addresses
- Flow Logs → CW Logs → CW Alarm → SNS
- Flow Logs → S3 → Athena → QuickSight

[](Untitled%20193d3d175d2380e2a3e1df829d357249.md)