# DMS - Database Migration Service

- quickly and securely migrate databases to AWS, resilient, self healing
- the source database remains available during the migration
- Supports:
    - Homogeneous migrations: ex Oracle to Oracle
    - Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC
- You must create an EC2 instance to perform the replication tasks

# DMS Sources and Targets

- Sources: on premises and ec2 instances database, s3, document db, rds, azure
- Targets: RDS, Redshift, DynamoDB, S3, OpenSearch Service, kinesis data streams, apache kafka,…

# AWS Schema Converion Tool (SCT)

- convert your database’s schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift

Source DB → DMS + SCT → Target DB (different engine)

- You do not need to use SCT if you are migrating the same DB engine
    - Ex: On-Premise PostgreSQL ⇒ RDS PostgreSQL

# DMS - Continuous Replication

- Need to install AWS SCT on computer on-premise ⇒ schema conversion
- Running AWS DMS Replication Instance (Fulload + CDC) ⇒ data migration

# AWS DMS - Multi-AZ Deployment

- When Multi-AZ Enabled, DMS provisions and maintains a synchronously stand replica in a different AZ
- Advantages
    - Provides Data Redundancy
    - Eliminates I/O freezes
    - Minimizes latency spikes