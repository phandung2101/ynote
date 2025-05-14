# AWS FSx Storage

Launch an 3rd party file system server on AWS

- FSx for Lustre
- FSx for NetApp ONTAP
- FSx for Window File Server
- FSx for OpenZFS

# FSx for Window File Server

- Support SMB & Window NTFS
- Require Microsoft Active Directory to work
- Scale up to 10s of GB/s, up to Milion of IOPS, 100s PB of data
- Can be mounted of Linux ( require SMB protocol )
- Storage option
    - SSD: latency sensitive workload
    - HDD: general workload
- Can be access from your on-premise infra (Direct Connect or VPN )
- Can be config to be Multi-AZ ( HA )
- Backup daily to S3

# FSx for Lustre

- Lustre - “Linux” and “cluster”
- Parallel distributed file system, for large-scale computing
- Machine Learning, HPC (High Performance Computing)
- Video process, Finance Modeling, Electronic Design Automation, …
- Scale up to 100s GB/s, milion of IOPS, sub ms latency
- Storage option:
    - SSD: latency sensitive
    - HDD: throughput sensitive
- **Seemless integration with S3**
    - Can “read S3” as file system (through FSx)
    - Can write output of computation to S3 (throguh FSx)
- Can be used on on-premise infra (Direct Connect or VPN)

# FSx for NetApp ONTAP

- Support protocol **NFS, SMB, iSCSI**
- Work with
    - **Window**
    - **Linux**
    - **MacOS**
    - VMWare Cloud on AWS
    - AWS ECS, EKS, EC2
    - Amazon Workspace & AppStream 2.0

# FSx for OpenZFS

- Support protocol NFS (v2,v3,v4.1,v4.2)
- Work with
    - **Window**
    - **Linux**
    - **MacOS**
    - VMWare Cloud on AWS
    - AWS ECS, EKS, EC2
    - Amazon Workspace & AppStream 2.0