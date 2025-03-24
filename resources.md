# Resources Deployed for Quant Research using AWS Batch

## Overview

This solution will use 4-6 CloudFormation stacks to deploy resources. If you do not select FSx or networking items then it will utilize less stacks.

Please monitor CloudFormation and youd IDE terminal for any errors.

## Core Stack

IAM Roles
- Container Image Build Role
  - "codebuild:BatchGetBuilds",
  - "codebuild:StartBuild",
  - "codebuild:StopBuild"
- Container Image Pipeline Role - On container image S3 buckets
  - "s3:Abort*",
  - "s3:DeleteObject*",
  - "s3:GetBucket*",
  - "s3:GetObject*",
  - "s3:List*",
  - "s3:PutObject",
  - "s3:PutObjectLegalHold",
  - "s3:PutObjectRetention",
  - "s3:PutObjectTagging",
  - "s3:PutObjectVersionTagging"
- Container Image Artifact Role


S3 Bucket
  - Container Image Pipeline Artifact Bucket
  - Container Image Pipeline Artifact Bucket Policy


GitHub Webhook
  Webhook in GitHub Repo specified within config.

CodeBuild Project
- Container Project Build Policy
  - "logs:CreateLogGroup",
  - "logs:CreateLogStream",
  -  "logs:PutLogEvents"
  -  "codebuild:BatchPutCodeCoverages",
  -  "codebuild:BatchPutTestCases",
  -  "codebuild:CreateReport",
  -  "codebuild:CreateReportGroup",
  -  "codebuild:UpdateReport"
  -  "s3:Abort*",
  -  "s3:DeleteObject*",
  -  "s3:GetBucket*",
  -  "s3:GetObject*",
  -  "s3:List*",
  -  "s3:PutObject",
  -  "s3:PutObjectLegalHold",
  -  "s3:PutObjectRetention",
  -  "s3:PutObjectTagging",
  -  "s3:PutObjectVersionTagging"
  -  "ecr:BatchCheckLayerAvailability",
  -  "ecr:BatchGetImage",
  -  "ecr:CompleteLayerUpload",
  -  "ecr:GetDownloadUrlForLayer",
  -  "ecr:InitiateLayerUpload",
  -  "ecr:PutImage",
  -  "ecr:UploadLayerPart"

Container Image Repo within AWS Elastic Container Repository


## Network CloudFormation Stack

Private VPC for Entire Stack, including PrivateLink Endpoints to Access AWS resources (S3, Batch, etc.)
- VPC with CIDR 172.0.0.0/16
- VPC Security Group "
- 1 VPC Private Subnet
- 2 VPC Route Tables
    - 1 for local only traffic
    - 1 for access to VPC PrivateLink Service Endpoints
- IAM Role 
  - AWSLambdaBasicExecutionRole Policy
  - Inline Policy
    - "ec2:AuthorizeSecurityGroupIngress",
    - "ec2:AuthorizeSecurityGroupEgress",
    - "ec2:RevokeSecurityGroupIngress",
    - "ec2:RevokeSecurityGroupEgress"
-  Ingress Security Group
  - HTTPS TCP 443 From VPC
  - ALL TCP From VPCEndpoint
- PrivateLink Endpoints - Internal Access to AWS Services
  - AWS Batch
  - AWS EC2
  - AWS SSm
  - AWS KMS
  - AWS CloudWatch Logs
  - AWS FSx for Luster
  - AWS ECS
  - AWS ECR
 
## S3 Storage CloudFormation Stack

- S3 bucket, unless specified buckets during implementation
  - S3 Bucket Policy that blocks and non-secure transport access

- Lambda Function for cleaning up temp S3 data
  - IAM Role for Lambda Execution
 
- S3 Directory Bucket


## Faster Throughput/IOPS Storage (If 

- S3 Bucket for use with FSx for Lustre
- 








