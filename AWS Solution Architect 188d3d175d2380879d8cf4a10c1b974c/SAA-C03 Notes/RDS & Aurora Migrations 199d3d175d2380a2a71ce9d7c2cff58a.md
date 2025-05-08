# RDS & Aurora Migrations

# RDS & Aurora MySQL Migrations

- RDS MySQL to Aurora MySQL
    - Option 1: DB Snapshots from RDS MySQL restored as MySQL Aurora DB
    - Option 2: Create an Aurora Read Replica from your RDS MySQL
- External MySQL to Aurora MySQL
    - Option 1:
        - Use Percona XtraBackup to create a file backup in S3
        - Create an Aurora MySQL DB from Amazon S3
    - Option 2:
        - Create an Aurora MySQL DB
        - Use the mysqldump utility to migrate MySQL into Aurora (slower than S3 Method)
- Use DMS if both databases are up and running

# RDS & Aurora PostgreSQL Migrations

- RDS PostgreSQL to Aurora PostgreSQL
    - Option 1: DB Snapshots from RDS PostgreSQL restored as PostgreSQL Aurora DB
    - Option 2: Create an Aurora Read Replica from your RDS PostgreSQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
- External PostgreSQL to Aurora PostgreSQL
    - Create a backup and put it in Amazon S3
    - Import it using the aws_s3 Aurora extension
- Use DMS if both databases are up and running