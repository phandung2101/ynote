# Instance Scheduler on AWS

- AWS solution deployed through CloudFormation (not a service)
- **Automaticaally start/stop your AWS services to reduce costs (up to 70%)**
- Example: stop company’s EC2 instances outside business hours
- **Supports EC2 instances, EC2 Auto Scaling Groups, and RDS instances**
- Schedules are managed in a DynamoDB table
- Uses resources’tags and Lambda to stop/start instances
- Supports cross-account and cross-region resources