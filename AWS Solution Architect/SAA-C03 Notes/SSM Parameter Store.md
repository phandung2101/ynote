# SSM Parameter Store

- Secure storage for config and secret
- Optional seamless encryption using KMS
- Serverless, scalable, durable, easy SDK
- Version tracking of configurations / secrets
- Security through IAM
- Noti with EventBridge
- Integration with CF

# SSM Parameter store hierachy

- using namespace like s3 to store key

![image.png](AWS%20Solution%20Architect/SAA-C03%20Notes/SSM%20Parameter%20Store%20188d3d175d238031aa10cfafb8662098/image.png)

- standard (4KB) vs advanced parameter tiers
- could have **parameters policies (advanced params)**
    - etc: Expiration, ExpirationNotification, NoChangeNotification, …
    - allow to assign a TTL to a params
    - can assign multiple polices at a time

# AWS Secrets Manager

- storing secrets ( just like params but more secured )
- force **rotation of secrets** every X days
- auto generation of secrets on rotation ( with Lambda)
- Integration with Amazon RDS
- Secrets are encrypted using KMS

***Exam tips: Secrets…integration… RDS or Aurora ⇒ think about Secrets Manager***

# AWS Secrets Manager - Multi-Region Secrets

- Replicate Secrets across multiple AWS Regions
- Secrets Manager keep read replicas in sync with the primary Secret
- Promote a read replica Secret → standalone Secret

***Use case: multi-region apps, disaster recoery strategies, multi-region DB…***