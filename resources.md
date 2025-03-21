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

Container Image Repo


## Network Stack


