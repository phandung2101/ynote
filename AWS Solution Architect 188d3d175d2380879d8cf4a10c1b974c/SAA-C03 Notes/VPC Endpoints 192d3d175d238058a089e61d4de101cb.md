# VPC Endpoints

# Private subnet to AWS Resource

- Using EC2 → NAT Gateway → Internet Gateway → AWS Resource
- EC2 → VPC Endpoint → AWS Resource

# VPC Endpoint (AWS PrivateLink)

- Every AWS service is publicly exposed (public URL)
- VPC Endpoints (powered by AWS PrivateLink) allows connect to AWS services using a **private network** instead of using the public Internet
- They’re redundant and scale horizontally
- They remove the need of IGW, NATGW,… to access AWS Services
- In case of issues:
    - Check DNS Setting Resolution in your VPC
    - Check Route Tables

# Types of Endpoint

- Interface Endpoint
    - Provisions an ENI (private IP address) as an entry point (must attach a Security Group)
    - Support most AWS services
    - $ per hours + $ per GB of data processed
- Gateway Endpoint
    - Provisions a gateway and must be used as a target in a route table (does not use security groups)
    - Supports both S3 and DynamoDB
    - Free

# Gateway or Interface Endpoint for S3?

- **Gateway is most likely going to be prefered all the time at the exam**
- Cost: free for Gateway, $ for interface endpoint
- Interface Endpoint is preferred access is required from on-premises (Site-to-site VPN or Direct Connect), a different VPC or a different region

# Noob note

| Interface Endpoint | Gateway Endpoint |
| --- | --- |
| Subnet-level | VPC-level |
| Using 1 or many ENIs | None |
| Using SG | Config Route Tables |
| All most every AWS service | S3, DynamoDB |