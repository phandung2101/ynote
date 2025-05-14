# Overview

# RPO and RTO

- RPO - Recover point object
- RTO - Recover time object

# Disaster recovery strategies ( From Lowest → Highest RTO )

- Backup and recovery
- Pilot light
- Warm standby
- Hot site/ Multiple site

## Backup and recovery

- very easy
- not too expensive
- High RPO, High RTO

## Pilot Light

- small version running on cloud (usually only running database replication)
- Lower RPO, Lower RTO than Backup and Recovery
- Only for critical application prevent data loss

## Warm standby

- all running at cloud with minimize scale
- Could scale quickly to production size
- Lower RPO, Lower RTO than Pilot Light

## Hot site/Multiple site

- running at cloud with production scale
- Quickly fail over to this
- Lowest RPO, Lowest RTO than all

# All region solution

- could using full cloud scaling
- Using Aurora to replication for all region

# Backup Recovery Tip

- Backup
    - EBS Snapshots, RDS automated backups / Snapshots, etc…
    - Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross Region Replication
    - From On-Premise: Snowball or Storage Gateway
- High Availability
    - Use Route53 to migrate DNS over from Region to Region
    - RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
    - Site-to-Site VPN as a recovery from Direct Connect
- Replication
    - RDS Replication (Cross Region), AWS Aurora + Global Databases
    - Database replication from on-premise to RDS
    - Storage Gateway
- Automation
    - CloudFormation / Elastic Beanstalk to re-create a whole new environment
    - Recover/ Reboot EC2 instances with CloudWatch if alarms fail
    - AWS Lambda functions to customized automations
- Chaos
    - Netflix has a “simian-army” randomly terminating EC2