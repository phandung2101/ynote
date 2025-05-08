# Storage Comparison

- **Object**
    - **S3**: General object storage.
    - S3 Glacier: Archival storage for long-term data.
- **Block Storage**
    - EBS: Persistent block storage for EC2.
    - Instance Storage: Temporary, high-performance block storage tied to EC2 instances.
- **File Storage**
    - EFS: Network file system for Linux with POSIX support.
    - FSx for Windows: Network file system for Windows servers.
    - FSx for Lustre: High-performance file system for Linux, often used in HPC.
    - FSx for NetApp ONTAP: Compatible with NetApp's ONTAP features, High OS Compatibility
    - FSx for OpenZFS: Managed ZFS file system.
- **Hybrid and Specialized Storage**
    - Storage Gateway: S3 & FSx File Gateway, Volume Gateway (cache & stored), Tape Gateway
    - Transfer Family: FTP, FTPS, SFTP interfaces for S3 or EFS.
    - DataSync: Schedule data sync from on-premises to AWS, or AWS to AWS
    - Snowcone / Snowball / Snowmobile: Physical devices to move large amount of data to the cloud