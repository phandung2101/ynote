# Session Manager

# SSM Session Manager

- Allows you to start a secure shell on your EC2 and on-premises servers
- **No SSH access, bastion hosts, or SSH keys needed**
- **No port 22 needed (better security)**
- Supports Linux, macOS, and Windows
- Send session log data to S3 or CloudWatch Logs

# Systems Manager - Run Command

- execute a document (=script) or just run a command
- Run command across multiple instances (using resource groups)
- No need for SSH
- Command Output can be shown in the AWS Console, sent to S3 bucket or CloudWatch Logs
- Send notifications to SNS about command status (in progress, success, failed, …)
- Integreated with IAM & CloudTrail
- Can be invoked using EventBridge

# System Manager - Patch Manager

- Automates the process of patching managed instances
- OS updates, applications updates, security updates
- Supports EC2 instances and on-premises servers
- Supports Linux, macOS, and Windows
- patch on-demand or on a schedule using **Maintenance Windows**
- Scan instances and generate patch compliance report (missing patches)

# Systems Manger - Maintenance Windows

- defines a schedule for when to perform actions on your instances
- Example: os patching, updating drivers, installing software,…
- Maintenance Windows contains
    - Schedule
    - Duration
    - Set of registered instances
    - Set of registered tasks

# Systems Manager - Automation

- Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
- Examples: restart instances, create an AMI, EBS snapshot
- **Automation Runbook -** SSM Documents to define actions preformed on your EC2 instances or AWS resources (pre-defined or custom)
- Can be triggered using:
    - Manually using AWS Console, AWS CLI or SDK
    - Amazon EventBridge
    - On a schedule using Maintenance Windows
    - By AWS Config for rules remediations