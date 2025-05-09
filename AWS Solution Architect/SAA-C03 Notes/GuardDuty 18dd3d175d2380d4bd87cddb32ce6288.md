# GuardDuty

- Intelligent Threat discovery to protect your AWS Account
- Use ML algo, anomaly detection, 3rd party data
- one click to enable (30day trial), no need install sw
- input data includes:
    - **CloudTrail Events Logs - unusual API calls, unauthorized deployments**
        - **Management Events -** create VPC subnet, create trail,…
        - **S3 Data Events** - get object, list object, delete object,…
    - **VPC Flow Logs -** unusual internal traffic, unusual IP address
    - **DNS Logs**  - compromised EC2 instances sending encoded data within DNS queries
    - **Optional Features** - EKS Audit Logs, RDS & Aurora, EBS, Lambda, S3 Data Event
- Can setup **EventBridge rules** to be notified in case of findings
- EventBridge rules can target AWS Lambda or SNS
- **Can protet against CryptoCurrency attacks (has a dedicated “finding” for it)**

# Keyword

- Machine Learning algorithm
- Intelligent Threat
- one click
- …