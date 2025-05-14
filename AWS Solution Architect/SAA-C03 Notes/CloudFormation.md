- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported)
- For example, within a CloudFormation template, you can setup
    - SG
    - 2 EC2 using this SG
    - S3 bucket
    - ELB in front of these machines
- Then CloudFormation creates those for you, in the **right order,** with the **exact configuration** that you specify

# Benefit

- Infrastructure as code
    - no resources are manually created, which is excellent for control
    - changes to the infrastructure are reviewed through code
- Cost
    - each resources within the stack is tagged with an identifier so you can easily see how much a stack costs you
    - you can estimate the costs of your resouces using the CloudFormation template
    - savings strategy: In dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely
- Productivity
    - Ability to destroy and re-create an infrastructure on the cloud on the fly
    - automated generation of Diagram for your templates!
    - declarative programming (no need to figure out ordering and orchestration)
- Don’t re-invent the wheel
    - Leverage existing templates on the web!
    - Leverage the documentation
- Supports (almost) all AWS resources:
    - Everything we’ll see in this course is supported
    - you can use “custom resources” for resources that are not supported

# Cloud Formation + Infrastructure Composer

- Example: WordPress CloudFormation Stack

- We can see all the **resources**
- We can see the **relations** between the components

# Cloud Formation - Service Role

- IAM role that allows CloudFormation to create/update/delete stack resources on your behalf
- Give ability to users to create/update/delete the stack resources even if they don’t have permissions to work with the resources in the stack
- Use cases:
    - You want to achieve the least privilege principle
    - But you don’t want to give the user all the required permissions to create the stack resources
- User must have **iam:PassRole** permissions