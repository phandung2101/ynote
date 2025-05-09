# AWS Backup

- fully managed service
- Centrally manage and automate backups across AWS services
- no need to create custom scripts and manual processes
- Supported services:
    - EC2 / EBS
    - S3
    - RDS (Al DBs engines) / Aurora / DynamoDB
    - DocumentDB / Neptune
    - EFS/FSx
    - Storage Gateway (Volume Gateway)
- Support cross-region backup
- Support cross-account backup
- Support PITR for supported services
- On-Demand and Scheduled backups
- Tag-based backup policies
- You create backup policies known as **Backup Plans**
    - Backup frequency (every 12 hours, daily, weekly, monthly, cron expression)
    - Backup window
    - Transition to Cold Storage (Never, Days, Weeks, Month, Years)
    - Retention Period (Always, Days, Weeks, Months, Years)

# AWS Backup Vault Lock

- enforce a WORM (Write once read many) state for all the backups that you store in your AWS Backup Vault
- Additional layer of defense to protect your backups against:
    - Inadvertent or malicious delete operations
    - Updates that shorten or alter retention periods
- Even the root user cannot delete backups when enabled