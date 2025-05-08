# KMS

# KMS Multi-region key

- use for multi region
- use case: encrypt region A → decrypt region B
- not global but is primary - replica
- multi-region key manage independently

# DynamoDB Global Tables or AuroraDB with KMS Multi-region Key client side encryption

- client encrypt sensitive data (using KMS MRK ) → save to dynamodb global tables
- client decrypt data by them self
- **protect sensitive data from adminstrator**

# S3 Replication Encryption Considerations

- Unencrypted and encrypted object with SSE-S3 are replication default
- SSE-C encrypted can be replicated
- Object encrypted with SSE-KMS → enable option:
    - bucket → KMS key
    - adapt KMS Key Policy for target key
    - IAM Role kms:Decrypt for the source KMS Key and kms:Encrypt for the target KMS Key
- **You can use multi-region KMS key, but they are  currently treat as independently keys by AWS S3 ( the object still be decrypted then encrypted)**

# AMI Sharing Process with KMS

1. AMI in Source Account is encrypted with KMS in source account
2. Must modify the image attribute to add a **Launch Permission** which corresponds to the specific target AWS account
3. Must share the KMS Key used to encrypted snapshot the AMI references with the target account / IAM role
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
5. When launching an EC2 instance from the AMI, optionally the target account can specifiy a new KMS key in its own account to re-encrypt the volumes

# Notes

- control access CMKs with **Key Access Policies**
- Customer Managed Keys mean user control create, delete, rotation key