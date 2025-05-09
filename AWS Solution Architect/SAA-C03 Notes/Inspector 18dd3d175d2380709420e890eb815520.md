# Inspector

- **Automated Security Assessments**

- **For EC2 instances**
    - leveraging the AWS System Manager (SSM) agent
    - analyze against unintended network accessibility
    - analyze the running OS against known vulnerabilities
- **For Container Images push to Amazon ECR**
    - Assessment of Container Images as they are pushed
- **For Lambda Functions**
    - Identifies sw vulnerabilities in function code and package dependencies
    - assessment of functions as they are deployed

- Reporting & integration with AWS Security Hub
- Send findings to Amazon Event Bridge

# Remember

- only for EC2 instances, Container Images & Lambda function
- continuous scanning of the infra, only when needed
- package vulnerabilities (ec2, lambda, ecr) - database of CVE
- network reachability (ec2)
- a risk score is associated with all vulnerabilities for priortization