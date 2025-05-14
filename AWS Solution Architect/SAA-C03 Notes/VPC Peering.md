# VPC Peering

- Private connect 2 VPCs using AWS Network
- Make them behave as if they were in the same network
- Must not have overlapping CIDRS
- VPC Peering connection is NOT transitive (must be established for each VPC that need to communicate with one another)

# Good to know

- you can create VPC Peering connection between VPCs in **different AWS accounts/regions**
- you can reference a security group in a peered VPC (works cross accounts - same region)