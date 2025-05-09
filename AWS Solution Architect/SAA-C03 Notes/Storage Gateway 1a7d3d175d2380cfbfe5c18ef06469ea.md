# Storage Gateway

# Description

## AWS Cloud Native Option

- Block: EBS, EC2
- File: EFS, FSx
- Object: S3, Glacier

## Strategy

AWS is pushing for “hybrid cloud”

- A part on AWS Cloud
- A part on On-premise

This can be due to:

- Security requirement
- Compliance requirement
- Long cloud migration
- IT Strategy

And hard part is S3 is proprietary storage technology (unlike EFS / NFS), so how do you expose S3 to on-premise?

**The answer is Storage Gateway**

# Storage Gateway

- Bridge between on-premise data and cloud data
- Use case:
    - Backup and restore
    - Tiered storage
    - On-premise cache and low-latency data access
    - Disaster Recovery
- Type:
    - S3 File Gateway
    - FSx File Gateway
    - Volume Gateway
    - Tape Gateway

# S3 File Gateway

- On-premise access data to Gateway with **NFS or SMB protocol**
- Most recent used data is cached on gateway
- Transition to Glacier with S3 bucket lifecycle transition
- Bucket access IAM role for each Gateway
- SMB intergation with AD (Active Directory) for authentication

# FSx File Gateway

- Native access for **FSx for Window File Server**
- **Local cache for most frequently access data**
- Window native compatibility (SMB, NTFS, Active Directory … )
- Useful for group file share and home directory

# Volume Gateway

- Support iSCSI protocol backed by S3
- Backed by EBS snapshot for on-premise restore
- **Cached volume: low latency access to most recent data**
- **Stored volume: entire dataset in on-premise and daily backup to S3**

# Tape Gateway

- Some company backup using physical tapes
- Virtual Tape Library (VTL) backed by S3 and Glacier
- Backup using tape-based process (and iSCSI interface)

# Storage Gateway - Hardware Appliance

- In normal gateway you will need on-premise virtualization
- Otherwise, you can use **Storage Gateway - Hardware Appliance** you can buy it on Amazon
- Work with Volume Gateway, File Gateway, Tape Gateway
- Helpful for daily NFS backup on small data center

![IMG_0039.jpeg](IMG_0039.jpeg)