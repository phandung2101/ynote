# EC2 Instance High Availability

# Create a highly available EC2 instance

- Public EC2 attached Elastic IP address
- Standby EC2 instance

When some issue happen ( monitored by CloudWatch Event  or Alarm based on metric) ⇒ start lambda function that do:

- Start the instance
- attach the elastic IP to standby EC2 instance
- Shutdown issue EC2

# Creating a highly available EC2 instance with an Auto Scaling Group

- Create ASG with setup
    - 1 min, 1 max, 1 desired, ≥ 2AZ
    - EC2 user data to attach The Elastic IP
    - EC2 instance role to Allow API calls to att

# Creating a highly available EC2 instance with ASG + EBS

- Same at previous scenario
    - Based on terminated ASG event hook ⇒ create EBS Snapshot + tags
    - Based on Lauch ASG event hook ⇒ Create EBS Volume from EBS Snapshot + ASG Launch lifecycle hook