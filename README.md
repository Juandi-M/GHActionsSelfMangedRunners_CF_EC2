## 1. General Overview of the CloudFormation Stack

This CloudFormation stack is designed to set up a robust AWS environment to host GitHub Runners for deploying and managing infrastructure. The stack includes a variety of AWS resources tailored to create a secure and scalable environment for continuous integration and deployment tasks.

### Resources Created:
- **VPC and Subnets**: Establishes a VPC with associated public subnets facilitating controlled access to AWS resources.
- **Internet Gateway and Routing**: Configures an Internet Gateway to provide Internet access to instances within public subnets.
- **EC2 Instances**: Provisions EC2 instances to serve as self-hosted GitHub Runners.
- **Security Groups**: Implements security rules that govern inbound and outbound traffic to the instances.
- **IAM Roles and Policies**: Sets up IAM roles with policies that grant the necessary permissions for the EC2 instances to interact with other AWS services.
- **S3 Buckets**: Includes an S3 bucket for storing application artifacts and managing state files securely.
- **DynamoDB Table**: Utilizes a DynamoDB table for storage related to the operations performed by the runners.
- **Additional Key Pairs**: Generates key pairs for secure SSH access to the instances.

## 2. GitHub Actions, Runners, and Tokens Interactions

### GitHub Actions:
GitHub Actions is a CI/CD platform that automates your software build, test, and deployment pipelines. Within this infrastructure, Actions are configured to deploy AWS resources through the runners hosted on EC2 instances.

### Runners:
The self-hosted runners are configured on EC2 instances. These runners listen for jobs from GitHub Actions, such as deploying updates or configuring infrastructure. The runners are set up using a user data script that installs necessary tools and configures each instance to connect securely with GitHub.

### Tokens:
Each runner uses a specific GitHub token to authenticate with GitHub and gain permissions to manage repository-specific actions. Tokens are stored securely and are used by runners to register with GitHub, ensuring that each runner can only interact with designated repositories.

## 3. Network Configuration

### Public and Private Subnets:
- **Public Subnets**: Used for resources that need to interact directly with the Internet, such as the EC2 instances configured as GitHub Runners.
- **Private Subnets** (optional enhancement): For added security, critical backend services can be hosted in private subnets that do not have direct Internet access.

### Internet Gateway:
- **Internet Gateway (IGW)**: Provides connectivity between the AWS resources in the public subnet and the Internet, enabling the EC2 instances to fetch and execute jobs from GitHub.

### Security Groups and NAT Gateway:
- **Security Groups**: Configured to allow only essential traffic. By default, allows inbound SSH traffic and outbound Internet access to handle GitHub interactions via HTTPS.
- **NAT Gateway** (optional): For instances in private subnets, a NAT Gateway facilitates outbound Internet access, allowing these instances to communicate with external services like GitHub without exposing them directly to the Internet.

### Monitoring:
- **AWS CloudWatch and VPC Flow Logs**: Used for monitoring traffic and detecting unusual activity, ensuring that the network remains secure and performs optimally.
