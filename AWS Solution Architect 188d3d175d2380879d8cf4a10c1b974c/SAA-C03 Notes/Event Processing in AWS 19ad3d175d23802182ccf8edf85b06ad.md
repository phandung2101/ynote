# Event Processing in AWS

# Lambda, SNS & SQS

- SQS → Lambda ( using with SQS DLQ)
- SQS FIFO → Lambda (sung with SQS DLQ)
- SNS → Lambda ( retries n time inside lambda ) → SQS (using for send DLQ)

# Fan Out Pattern: deliver to multiple SQS

Scenario: want to send to multiple SQS queue

- Option 1: Send per queue ( with big n of queues this is such)
- Option 2: Send to single SNS → fanout to multiple SQS

# S3 Event Notifications

- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication….
- Object name filtering possible (*.jpg)
- Use case: generate thumbnails of images uploaded to S3
- **Can create as many “S3 events” as desired**
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer

# S3 Event Notifications with Amazon EventBridge

- **Advanced filtering** options with JSON rules (metadata, object size, name…)
- **Multiple Destinations** - ex Step Functions, Kinesis Streams / Firehose…
- **EventBridge Capabilities** - Archive, Replay Events, Reliable delivery

# Amazon EventBridge - Intercept API Calls

User → DeleteTable API Call → DynamoDB → CloudTrail → EventBridge → SNS

- CloudTrail can log any API call

# API Gateway - AWS Service Integration Kinesis Data Streams example

Client → API Gateway → Kinesis Data Streams → Kinesis Data Firehose → S3