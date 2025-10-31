# AWS Native AI Least Privlege
This project is designed to implement a granular, least privlege strategy for Amazon Bedrock. This solution will enforce strict, multi-layered access controls across the entire model lifecycle, from model selection and customization to final operation, to enhance security and operational efficiency.

## Components
1. BedrockSecurityFramework.yaml
2. SCP.yaml

## Requirements

1. After the BedrockSecurityFramework Cloudformation template is deployed, you must enable model invocation logging in amazon Bedrock:
    - Navigate to Bedrock in console, go to the settings tab.
    - In settings, select Enable Model Invocation logging.
    - Select Cloudwatch logs only, then use the log group created from the cloudformation template /aws/bedrock/model-invocation-logging
    - For service role, use the role created from the cloudformation template "Bedrock-LoggingServiceRole".
    - Save Settings.

2. For the SCP.yaml, this will be deployed in the organizations management account. Once deployed, the user will manually attach this SCP to the organizational unit where the member account is located.

## What the solution deploys

BedrockSecurityFramework.yaml
1. One Bedrock Guardrail (BedrockExecutionGuardrail) for Bedrock model invocations with content filtering and prompt attack protection.
2. DevelopmentRole IAM role for bedrock model testing and model customizaiton jobs.
3. ExecutionRole IAM role for model testing and executions.
4. Bedrock Interface VPC Endpoint to ensure that bedrock API calls originate from private network. The VPC Endpoint also restricts usage to the Development and Execution roles only.
5. Bedrock service role (BedrockS3ServiceRole) which is used for bedrock to interact with S3 buckets and retrieve training data.
6. Cloudwatch log group for bedrock model invocation logs - /aws/bedrock/model-invocation-logging.
7. A KMS key which is used for encrypting the model invocation cloudwatch logs.
8. Bedrock service role to be used for interaction between bedrock and cloudwatch logs, with regards to model invocation logging.
9. IAM Access Analyzer, which is used for monitoring internal access of development and execution roles.

SCP.yaml
1. A service control policy which defines bedrock models which are authorized for usage.
