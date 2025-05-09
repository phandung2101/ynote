# Certificate Manager (ACM)

- easily provision, manage, and deploy ***TLS Certificates***
- auto renewal
- integrations with
    - ELB
    - CF distributions
    - APIs on API Gateway
- cannot use ACM with EC2 ( can’t be extracted )

# Requesting Public Cert

1. List domain names to be included in the certificate
    1. fully name ( abc.dung.com ….) 
    2. wildcard domain: *.dung.com
2. Select validation method: DNS or Email validation
    1. DNS Validation is preferred for automation purposes
    2. Email validation will send emails to contact addresses in the WHOIS database
    3. DNS Validation will leverage CNAME record
3. Few hours to verified
4. The Public Certificate will be enrolled for automatic renewal

# Importing Public Certificate

- no auto renewal, must import a new certificate before expiry
- **ACM send daily expiration events** starting 45 days prior to expiration
- **AWS Config** has a managed rule ‘acm-certificate-expiration-check’ to check for expiring certificates ( configuratable number of days)

# API Gateway - Endpoint Type

- Edge-Optimized (default): For global clinet
    - req are routed through the CF Edge locations (improves latency)
    - The API Gateway still lives in only one region
- Regional
    - same region
    - manual combine with CloudFront
- Private:
    - Accessed from your VPC using an interface VPC Endpoint (ENI)
    - Use a resource policy to define access

# Notes

- if using CloudFront, certificates must be created in ***us-east-1 .***
-