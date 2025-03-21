# Quant Research using AWS Batch

## Overview

This project deploys an AWS Batch infrastructure for Quant Research.

## Pre-requisites

The project has been built and tested using AWS Linux 2023 `x86` architecture

If deploying on you local computer, you will need [Python](https://www.python.org/downloads/), [JQ](https://jqlang.org/download/), [Docker](https://docs.docker.com/get-started/get-docker/), [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), and curl installed as well.

a. AWS CDK - `v2.177.0`

 ```shell
 # Install NVM
 curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

 # Reinitialize your Shell
 source ~/.bash_profile

 # Install Node LTS
 nvm install --lts

 # Install AWS CDK
 npm install -g aws-cdk@2.177.0
 ```

b. Copy this repo via [GitHub Fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) to your own GitHub account. 

c. Create a GitHub Personal Access Token for allowing AWS CodePipeline to access the repository and build the container image. Refer the [GitHub documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token). You need to provide below permissions:
- Select radio button next to "Only Select Repositories" and select the forked repo from the drop-down
- Read access to code, commit statuses, and metadata
- Read and Write access to repository webhooks
- Copy token id. Should start with "github_pat_"

## Deployment

1. Export AWS credentials via CLI for your target environment
2. Create an AWS Secret with the GitHub personal access token value for triggering deployment via CI/CD for your
   application code
   ```shell
   aws secretsmanager create-secret \
   --name github-token \
   --description "GitHub PAT for the repository" \
   --secret-string "<GITHUB_PAT>"
   ```
3. Clone repo to local computer
   ```shell
   git clone https://github.com/<your github account>/<forked repo name>
   ```
4. Update the [.env](infrastructure/.env) file with the values for the infrastructure deployment. Below are the
   placeholder values provided in the [.env](infrastructure/.env) file for reference. Update the following items:
- AWS_ACCOUNT_ID
- AWS_ACCOUNT_ID
- GITHUB_OWNER
- GITHUB_REPO
- GITHUB_TOKEN_SECRET_NAME (from step 2 above)
- NAMESPACE
   ```shell
   # AWS account id and region where you need to deploy
   AWS_ACCOUNT_ID=<012345678901>
   AWS_REGION=<us-east-1>
   
   GITHUB_OWNER=<github-account-owner>
   GITHUB_REPO=<github-repo-name>
   
   # Provide the secret name in AWS Secrets Manager which stores your GitHub Personal Access Token
   GITHUB_TOKEN_SECRET_NAME=<github-token>
   
   # Unique identifier used as a prefix for your AWS infrastructure resources
   NAMESPACE=<quant-research-with-batch>
   ```
6. If needed, modify the configurations provided in the [parameters.json](infrastructure/config/parameters.json)
   | Line/Section | Item | Value | Why Change? |
   | -------- | -------- | -------- | -------- |
   | 6 | FSx Storage | True / False (default) | Change to false if high throughput is not needed |
   | 63 | spot | True / False (default) | Change to True to use Spot instances |
   | 71 | S3 Bucket Access | Add ARN's for S3 Buckets the Batch process can have access to | The solution has no S3 access without adding buckets to this section |
     
8. Build the Python virtual environment
   ```shell
   python -m venv .venv
   source .venv/bin/activate
   cd infrastructure
   pip install -U -r requirements.txt
   cdk bootstrap
   cdk deploy --all --require-approval never
   ```


## Using the environment

Stay tuned

## Clean up

1. Delete all the stacks
   ```shell
   source .venv/bin/activate
   cd infrastructure
   cdk destroy --all
   ``` 
2. Delete the GitHub token secret stored in AWS Secrets Manager
3. You may need to manually delete resources like Amazon S3, if they contain data

# Security

See [CONTRIBUTING](./CONTRIBUTING.md#security-issue-notifications) for more information.

Note: this asset represents a proof-of-value for the services included and is not intended as a production-ready solution. You must determine how the AWS Shared Responsibility applies to their specific use case and implement the needed controls to achieve their desired security outcomes. AWS offers a broad set of security tools and configurations to enable out customers.

# License

This library is licensed under the MIT-0 License. See the [LICENSE](./LICENSE) file.
