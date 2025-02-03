# FloTorch CloudFormation Templates

Welcome to FloTorch! This guide will help you set up FloTorch's infrastructure on AWS. Whether you're new to cloud computing or an experienced developer, we'll guide you through each step.

## Before You Begin (Prerequisites)

### 1. AWS Account and Tools
1. AWS Account with administrative access
2. AWS CLI installed and configured on your computer
3. AWS Marketplace Subscription (see next section)

### 2. AWS Marketplace Subscription
Before starting the installation, subscribe to FloTorch:

1. Visit the [FloTorch AWS Marketplace page](https://aws.amazon.com/marketplace/pp/prodview-z5zcvloh7l3ky?ref_=aws-mp-console-subscription-detail-payg)
2. Click on the "View Purchase options" button
3. After subscribing, click on 'Continue to Configuration'
4. Select your preferred fulfillment option and Software Version

The CloudFormation templates will automatically use your FloTorch subscription to set up all required resources.

### 3. Required AWS Service Quotas
Your AWS account needs sufficient quotas for these services:
- VPC and related networking resources
- Lambda functions
- EventBridge rules
- SageMaker endpoints
- OpenSearch domains
- DynamoDB tables
- ECR repositories
- AppRunner services

### 4. Required AI Model Access
Your AWS account needs access to these Bedrock models:

**For Embedding:**
- Amazon/amazon.titan-embed-text-v2:0
- Amazon/amazon.titan-embed-image-v1
- Cohere/cohere.embed-english-v3
- Cohere/cohere.embed-multilingual-v3

**For Retrieval:**
- Amazon/amazon.titan-text-lite-v1
- Amazon/amazon.titan-text-express-v1
- Amazon/amazon.nova-lite-v1:0
- Amazon/amazon.nova-micro-v1:0
- Amazon/amazon.nova-pro-v1:0
- Anthropic/anthropic.claude-3-5-sonnet-20241022-v2:0
- Anthropic/anthropic.claude-3-5-sonnet-20240620-v1:0
- Cohere/cohere.command-r-plus-v1:0
- Cohere/cohere.command-r-v1:0
- Meta/meta.llama3-2-1b-instruct-v1:0
- Meta/meta.llama3-2-3b-instruct-v1:0
- Meta/meta.llama3-2-11b-instruct-v1:0
- Meta/meta.llama3-2-90b-instruct-v1:0
- Mistral AI/mistral.mistral-7b-instruct-v0:2
- Mistral AI/mistral.mistral-large-2402-v1:0

### 5. Required AWS Permissions
You'll need either:
- Administrative access (recommended), or
- The following specific permissions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "s3:*",
                "ec2:*",
                "iam:*",
                "lambda:*",
                "dynamodb:*",
                "events:*",
                "sagemaker:*",
                "opensearch:*",
                "ecr:*",
                "apprunner:*",
                "cloudwatch:*",
                "logs:*",
                "ssm:*",
                "es:*"
            ],
            "Resource": "*"
        }
    ]
}
```

## What You're Installing

FloTorch creates a comprehensive architecture for managing and deploying machine learning models, combining serverless components with managed services like OpenSearch. Here's what you get:

### Core Infrastructure Components

- **VPC Stack**: Your private network infrastructure
- **VPC Endpoint Stack**: Secure access to AWS services within the VPC
- **DynamoDB Stack**: Database tables and S3 bucket for data storage
- **OpenSearch Stack**: Search functionality
- **ECR Repository Stack**: Container image repositories
- **ECS Stack**: Elastic Container Service for running containerized applications
- **Lambda Stack**: Serverless functions for various operations
- **State Machine Stack**: Step Functions for orchestration
- **AppRunner Stack**: Application hosting and management

### Key Features

1. Automated deployment and management
2. Serverless architecture for cost optimization
3. Secure VPC configuration
4. Integrated monitoring and logging
5. Automated container image management

## Installation Guide

### Option 1: Quick Install (Recommended for Beginners)

1. Click this link: [Install FloTorch (US East 1)](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create?stackName=flotorch-stack&templateURL=https://flotorch-public.s3.us-east-1.amazonaws.com/2.0.1/templates/master-template.yaml)

2. Fill in these details in the CloudFormation console:
   - **ProjectName**: your-project-name
     (Example: "flotorch")
   - **TableSuffix**: unique-suffix
     (Example: "abc123" - must be 6 lowercase characters)
   - **ClientName**: your-client-name
     (Example: "acmecorp" - must be lowercase)
   - **OpenSearchAdminUser**: admin
     (Example: "admin")
   - **OpenSearchAdminPassword**: YourSecurePassword123!
     (Must be 8-41 characters with letters, numbers, and symbols)
   - **NginxAuthPassword**: YourNginxPassword123!
     (Must be 8-41 characters with letters, numbers, and symbols)

3. Click "Review and create" and acknowledge that the stack will create IAM resources

### Option 2: Command Line Installation

For advanced users, you can deploy using the AWS CLI:

```bash
aws cloudformation create-stack \
    --stack-name flotorch-stack \
    --template-url https://flotorch-public.s3.us-east-1.amazonaws.com/templates/master-template.yaml \
    --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
    --parameters \
        ParameterKey=ProjectName,ParameterValue=your-project-name \
        ParameterKey=TableSuffix,ParameterValue=unique-suffix \
        ParameterKey=ClientName,ParameterValue=your-client-name \
        ParameterKey=OpenSearchAdminUser,ParameterValue=admin \
        ParameterKey=OpenSearchAdminPassword,ParameterValue=YourSecurePassword123! \
        ParameterKey=NginxAuthPassword,ParameterValue=YourNginxPassword123!
```

## Understanding the Templates

Each template serves a specific purpose in your FloTorch infrastructure:

### master-template.yaml
- Main template that controls everything
- Manages dependencies between stacks
- Handles global parameters

### lambda-template.yaml
- Contains Lambda functions for:
  - SageMaker endpoint cleanup
  - Docker image management
  - Runtime operations
  - Cost computation

### vpc-template.yaml
- Creates VPC and associated networking resources
- Sets up public and private subnets
- Configures route tables and internet gateway

### dynamodb-template.yaml
- Sets up DynamoDB tables for data storage
- Configures auto-scaling for tables
- Implements backup and recovery options

### opensearch-template.yaml
- Deploys OpenSearch domain
- Configures access policies and encryption
- Sets up index templates and mappings

### ecr-template.yaml
- Creates Elastic Container Registry repositories
- Sets up lifecycle policies for images
- Configures cross-account access if needed

### apprunner-template.yaml
- Deploys AppRunner services
- Configures auto-scaling and health checks
- Sets up custom domains and SSL certificates

## Cost Overview

Here's what you can expect to pay per day in the US East 1 region:

1. **Lambda Functions**: $0.50-$2.00/day
   - Free tier: 1M requests/month
   - 400,000 GB-seconds of compute time included

2. **SageMaker Endpoints**: $1.40-$2.80/day
   - Using ml.t3.medium instances
   - Based on 1-2 endpoints running

3. **OpenSearch**: $41.81/day
   - r7g.2xlarge instance: $0.5184/hour
   - Provisioned IOPS (16000): $1.152/hour
   - Provisioned Throughput: $0.072/hour

4. **DynamoDB**: $2-$5/day
   - Write requests: $1.25 per million
   - Read requests: $0.25 per million
   - Storage: $0.25 per GB per month

5. **AppRunner**: $1.54-$3.08/day
   - 1 vCPU, 2GB memory: $0.064/hour
   - Based on 1-2 instances

6. **ECR**: $0.20-$0.50/day
   - Storage: $0.10/GB/month
   - Data transfer: $0.09/GB

7. **VPC and Networking**: $1.08-$2.00/day
   - NAT Gateway: $0.045/hour
   - Data processing: $0.045/GB

8. **CloudWatch**: $0.50-$1.50/day
   - Log ingestion: $0.30 per GB
   - Metrics: $0.01 per 1,000 requests

9. **Step Functions**: $0.50-$2.00/day
   - $0.025 per 1,000 state transitions

10. **ECS (Fargate)**: $2.00-$5.00/day
    - vCPU: $0.04048 per hour
    - Memory: $0.004445 per GB-hour

11. **Bedrock**: $5.00-$10.00/day
    - Input tokens: $0.0001 per 1K
    - Output tokens: $0.0002 per 1K

**Total Estimated Cost**: $56.53-$75.69/day
(Note: Actual costs depend on your usage)

## Monitoring Your Installation

### 1. CloudWatch Metrics
Monitor:
- Lambda execution
- SageMaker endpoint usage
- AppRunner service metrics

### 2. Logs
Check:
- Lambda function logs
- AppRunner application logs
- OpenSearch logs

### 3. Alerts
Set up for:
- Cost thresholds
- Error rates
- Service health

## Security Features

Your FloTorch installation includes:
1. VPC isolation
2. IAM role-based access
3. Encryption at rest
4. Network security groups
5. NGINX basic authentication
6. OpenSearch access control

## Troubleshooting Guide

### Installation Issues
If your stack creation fails:
1. Check CloudFormation events
2. Verify IAM permissions
3. Validate parameter values

### Runtime Problems
If you encounter issues after installation:
1. Check CloudWatch logs
2. Verify network connectivity
3. Check service quotas

## Getting Help

Need assistance? Here's how to get help:
1. Check CloudWatch logs
2. Review stack events
3. Contact our DevOps team
4. Email us at info@flotorch.ai

## Contributing

Want to help improve FloTorch?
1. Fork the repository
2. Create a feature branch
3. Submit a pull request
