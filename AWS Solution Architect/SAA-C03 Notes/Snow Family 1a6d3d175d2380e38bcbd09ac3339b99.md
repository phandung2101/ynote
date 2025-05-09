# Snow Family

- Highly secure and portable
- Migrate data in and out AWS
- Collect and process data at the edge

# Challenge with internet migration

- Limited connectivity
- Limited network bandwidth
- Shared bandwidth
- Network cost
- Connection stability

***When transfer data that take over 1 week, consider using snowball devices!***

# Snow Family process

1. Request device
2. Got device
3. Migrate data offline
4. Send back to AWS
5. AWS Migrate to your target service (S3,…)

**Or it is opposite when export data from AWS**

# AWS Snowball

- Snowball Edge Storage Optimize
    - 80TB HDD for block volume and S3 compatible with object storage
- Snowball Edge Compute Optimize
    - 42TB HHD or 28TB NVMe capacity for block volume and S3 compatible with object storage

**Use case: large data migration, DC decommision, disaster recovery**

# AWS Snowcone & Snowcone SSD

- Small, portable computing, anywhere, rugged & secure,
withstands harsh environments
- Light
- Use for edge computing, storage, and data transfer
- **Snowcone - 8TB of HDD storage**
- **Snowcone SSD - 14TB of SSD storage**
- Use when Snowball not fit (space constraint)
- Can sent it back to AWS or connect internet and send data over internet (AWS DataSync)

# AWS OpsHub

Software that install in your device/laptop to control snow device

# Solution Architecture: Snowball to Glacier

- **Snowball cannot import to Glacier directly**
- You must migrate to S3 bucket first
- Then use S3 Lifecycle policy to tranfer to Glacier