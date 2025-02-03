# FloTorch CloudFormation Templates

Welcome to FloTorch! This guide will help you set up FloTorch's infrastructure on AWS. Whether you're new to cloud computing or an experienced developer, we'll guide you through each step.

## Before You Begin (Prerequisites)

### 1. AWS Account and Tools

| Requirement                  | Description                               |
| ---------------------------- | ----------------------------------------- |
| AWS Account                  | Must have administrative access           |
| AWS CLI                      | Installed and configured on your computer |
| **AWS Marketplace Subscription** | Required before installation              |

### 2. AWS Marketplace Subscription :bangbang:

Before starting the installation, subscribe to FloTorch:

1. Visit the [FloTorch AWS Marketplace page](https://aws.amazon.com/marketplace/pp/prodview-z5zcvloh7l3ky?ref_=aws-mp-console-subscription-detail-payg)
2. Click on the "View Purchase options" button
3. After subscribing, click on 'Continue to Configuration'
4. Select your preferred fulfillment option and Software Version

### 3. Required AWS Permissions

| Permission Type     | Description                                                                                                               |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Full Access**     | Recommended (Administrative Access)                                                                                       |
| **Custom Policies** | CloudFormation, S3, EC2, IAM, Lambda, DynamoDB, EventBridge, SageMaker, OpenSearch, ECR, AppRunner, CloudWatch, Logs, SSM |

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:*", "s3:*", "ec2:*", "iam:*", "lambda:*",
                "dynamodb:*", "events:*", "sagemaker:*", "opensearch:*", "ecr:*",
                "apprunner:*", "cloudwatch:*", "logs:*", "ssm:*", "es:*"
            ],
            "Resource": "*"
        }
    ]
}
```

### 4. Required AWS Service Quotas

| AWS Service | Quotas Needed        |
| ----------- | -------------------- |
| VPC         | Networking resources |
| Lambda      | Functions            |
| EventBridge | Rules                |
| SageMaker   | Endpoints            |
| OpenSearch  | Domains              |
| DynamoDB    | Tables               |
| ECR         | Repositories         |
| AppRunner   | Services             |

### 5. Required AI Model Access

| Use Case      | Model Name                                                                                                                                                                                                                                                                                                                                |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Embedding** | Amazon/amazon.titan-embed-text-v2:0, Amazon/amazon.titan-embed-image-v1, Cohere/cohere.embed-english-v3, Cohere/cohere.embed-multilingual-v3                                                                                                                                                                                              |
| **Retrieval** | Amazon/amazon.titan-text-lite-v1, Amazon/amazon.titan-text-express-v1, Amazon/amazon.nova-lite-v1:0, Amazon/amazon.nova-micro-v1:0, Amazon/amazon.nova-pro-v1:0, Anthropic/anthropic.claude-3-5-sonnet-20241022-v2:0, Cohere/cohere.command-r-plus-v1:0, Meta/meta.llama3-2-90b-instruct-v1:0, Mistral AI/mistral.mistral-large-2402-v1:0 |

## What You're Installing

### Core Infrastructure Components

| Component            | Description                        |
| -------------------- | ---------------------------------- |
| VPC Stack            | Private network infrastructure     |
| VPC Endpoint Stack   | Secure access to AWS services      |
| DynamoDB Stack       | Database tables and S3 bucket      |
| OpenSearch Stack     | Search functionality               |
| ECR Repository Stack | Container image repositories       |
| ECS Stack            | Running containerized applications |
| Lambda Stack         | Serverless functions               |
| State Machine Stack  | Step Functions for orchestration   |
| AppRunner Stack      | Application hosting                |

### Key Features

1. Automated deployment and management
2. Serverless architecture for cost optimization
3. Secure VPC configuration
4. Integrated monitoring and logging
5. Automated container image management

## Cost Overview

| Service                  | Approx Cost (per day)   |
| ------------------------ | ----------------------- |
| Lambda Functions         | \$0.50-\$2.00           |
| SageMaker Endpoints      | \$1.40-\$2.80           |
| OpenSearch               | \$41.81                 |
| DynamoDB                 | \$2-\$5                 |
| AppRunner                | \$1.54-\$3.08           |
| ECR                      | \$0.20-\$0.50           |
| VPC and Networking       | \$1.08-\$2.00           |
| CloudWatch               | \$0.50-\$1.50           |
| Step Functions           | \$0.50-\$2.00           |
| ECS (Fargate)            | \$2.00-\$5.00           |
| Bedrock                  | \$5.00-\$10.00          |
| **Total Estimated Cost** | **\$56.53-\$75.69/day** |

## Monitoring Your Installation

| Monitoring Type    | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| CloudWatch Metrics | Lambda execution, SageMaker usage, AppRunner service metrics |
| Logs               | Lambda logs, AppRunner logs, OpenSearch logs                 |
| Alerts             | Cost thresholds, Error rates, Service health                 |

## Troubleshooting Guide

| Issue Type              | Steps to Resolve                                                               |
| ----------------------- | ------------------------------------------------------------------------------ |
| **Installation Issues** | Check CloudFormation events, Verify IAM permissions, Validate parameter values |
| **Runtime Problems**    | Check CloudWatch logs, Verify network connectivity, Check service quotas       |

## Getting Help

| Support Option      | Contact Method                               |
| ------------------- | -------------------------------------------- |
| Check Logs          | CloudWatch logs                              |
| Review Stack Events | CloudFormation Console                       |
| DevOps Team         | Internal support                             |
| Email Support       | [info@flotorch.ai](mailto\:info@flotorch.ai) |

## Contributing

Want to help improve FloTorch?

1. Fork the repository
2. Create a feature branch
3. Submit a pull request
