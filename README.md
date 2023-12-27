## Overview of the CloudFormation Template
This Cloudformation template is used to create WAF for CloudFront and to analyze the WAF logs using the Athena

### Parameters
Prefix: A string used to uniquely identify resources.

LogBucketName: The name of the S3 bucket for storing access logs.

WebBucketName: The name of the S3 bucket for hosting the web content.

AthenaDB: The name of the Glue database for Athena.

AthenaTable: The name of the Glue table for Athena.

### Resources
1. AccessLogBucket (S3 Bucket)
Purpose: Store access logs securely.
Configuration: Private access control.

2. RuleGroup (WAFv2 Rule Group)
Purpose: Implement georestriction rules for web traffic.
Configuration: Block requests from Japan (JP).

3. WebACL (WAFv2 Web Access Control List)
Purpose: Define access control rules for CloudFront.
Configuration: Reference the RuleGroup for georestriction.

5. LoggingConfiguration (WAFv2 Logging Configuration)
Purpose: Configure logging for the WebACL.
Configuration: Log to the AccessLogBucket.

6. Bucket (S3 Bucket)
Purpose: Host web content securely.
Configuration: Private access control.

7. OAI (CloudFront Origin Access Identity)
Purpose: Control access to S3 content from CloudFront.

8. Distribution (CloudFront Distribution)
Purpose: Distribute web content globally.
Configuration: Uses the defined Bucket and WebACL.

9. BucketPolicy (S3 Bucket Policy)
Purpose: Grant CloudFront access to the S3 Bucket.
Configuration: Grants read access to CloudFront.

10. Function (Lambda Function)
Purpose: Automate S3 object creation/deletion.
Configuration: Creates 'index.html' in the S3 Bucket.

11. FunctionRole (IAM Role for Lambda)
Purpose: Define permissions for the Lambda function.
Configuration: Grants necessary S3 permissions.

12. CustomResource (Custom CloudFormation Resource)
Purpose: Trigger the Lambda function during CloudFormation stack operations.

13. GlueDatabase (AWS Glue Database)
Purpose: Store metadata about Athena tables.
Configuration: Uses the specified AthenaDB.

15. TransactionsTable (AWS Glue Table)
Purpose: Define a table for Athena to analyze.
Configuration: Includes parameters and storage location.

### Outputs
CloudFrontURL: The URL of the CloudFront distribution.

## Conclusion
This CloudFormation template provides a comprehensive infrastructure for securing, logging, and analyzing web traffic. 
By leveraging AWS services such as WAF, CloudFront, S3, Lambda, and Glue, you can create a scalable and secure environment for your web applications. 
Feel free to customize the template to fit your specific requirements and enhance the security and analysis capabilities of your web infrastructure on AWS.
