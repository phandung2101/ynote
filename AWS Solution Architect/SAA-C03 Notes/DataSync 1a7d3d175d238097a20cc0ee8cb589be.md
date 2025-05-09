# DataSync

- **Move large amount of data using ONLINE connectivity** to and from:
    - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API…) - needs agent
    - AWS to AWS (different storage services) - no agent needed
- Can synchronize to:
    - S3
    - EFS
    - FSx
- Replication tasks can be scheduled hourly, daily, weekly
- **File permissions and metadata are preserved** (NFS POSIX, SMB…)
- One agent task can use 10 Gbps, **can setup a bandwidth limit**

# AWS DataSync NFS/SMB to AWS (S3, EFS, FSx…)

Can use AWS DataSync between 2 services in S3, EFS, FSX to copy data and metadata