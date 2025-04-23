# AWS User Management CloudFormation Project

## Overview
This project provides CloudFormation templates for automating the creation and management of AWS IAM users, groups, and their associated credentials. It demonstrates secure credential management practices using AWS Secrets Manager and Parameter Store, along with event-driven notifications using EventBridge and Lambda.

## Project Structure
- **lab1.yaml**: The main CloudFormation template that defines all AWS resources
- **deployment.yaml**: A configuration file for deployment tools that references the main template

## Architecture
![Architecture Diagram](https://via.placeholder.com/800x400?text=AWS+User+Management+Architecture)

The solution implements the following architecture:
- IAM Users and Groups with specific permissions
- Secure credential storage using AWS Secrets Manager and Parameter Store
- Event-driven notifications using EventBridge and Lambda
- Automated user creation notification system

## Resources Created

### IAM Resources
- **IAM Users**: Three users (User1, User2, User3) with login profiles
- **IAM Groups**:
  - `S3ReadOnlyGroup`: Provides read-only access to S3 objects
  - `EC2S3ReadOnlyGroup`: Provides read-only access to S3 objects (despite the name suggesting EC2 access)
- **IAM Role**: Lambda execution role with permissions to access Parameter Store and Secrets Manager

### Credential Management
- **Parameter Store**: Stores user email addresses
- **Secrets Manager**: Generates and stores secure one-time passwords for users

### Event-Driven Components
- **Lambda Function**: 
  - Retrieves user emails from Parameter Store
  - Fetches generated passwords from Secrets Manager
  - Logs the user credentials for administrative purposes
  - Written in Python 3.9
- **EventBridge Rule**: Monitors for IAM user creation events and triggers the Lambda function automatically

## Prerequisites
- AWS CLI installed and configured
- Appropriate AWS permissions to create IAM resources, Lambda functions, EventBridge rules, etc.
- Python 3.9 (for Lambda function execution)

## Deployment Instructions

### Using AWS CLI
1. Deploy the CloudFormation stack:
   ```bash
   aws cloudformation deploy --template-file lab1.yaml --stack-name user-management-stack --capabilities CAPABILITY_NAMED_IAM
   ```

### Using Deployment Configuration
1. Update the `deployment.yaml` file if you need to add parameters or tags
2. Deploy using your preferred deployment tool that supports this configuration format

## Security Considerations
- The template generates secure one-time passwords for users
- Users are required to reset their passwords on first login
- Credentials are stored securely in AWS Secrets Manager and Parameter Store
- The Lambda function has minimal required permissions following the principle of least privilege
- Consider adding additional security measures such as MFA for the created users

## Usage Examples

### Checking User Credentials
The Lambda function will automatically log user credentials when a new user is created. You can also manually invoke the Lambda function:

```bash
aws lambda invoke --function-name UserNotificationLambda output.txt
```

### Adding New Users
To add more users, modify the CloudFormation template to include additional user resources following the same pattern.

## Customization
- Modify the IAM policies to adjust permissions as needed
- Add additional EventBridge rules to trigger on different IAM events
- Extend the Lambda function to send notifications via SNS or other services

## Troubleshooting
- Check CloudWatch Logs for Lambda function execution logs
- Verify that the EventBridge rule is correctly configured to capture IAM events
- Ensure that the Lambda function has the necessary permissions to access Parameter Store and Secrets Manager


