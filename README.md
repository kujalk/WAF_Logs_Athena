## Overview of the CloudFormation Template
This Cloudformation template is used to create WAF for CloudFront and to analyze the WAF logs using the Athena

### Parameters
| Parameter               | Description                                             |
|-------------------------|---------------------------------------------------------|
| Prefix                  | A string used to uniquely identify resources.            |
| LogBucketName           | The name of the S3 bucket for storing access logs.       |
| WebBucketName           | The name of the S3 bucket for hosting the web content.   |
| AthenaDB                | The name of the Glue database for Athena.                |
| AthenaTable             | The name of the Glue table for Athena.                   |

### Resources created
| Resource Name           | Purpose                                               | Configuration                                  |
|-------------------------|-------------------------------------------------------|------------------------------------------------|
| AccessLogBucket         | S3 Bucket                                             | Store access logs securely.                    |
| RuleGroup               | WAFv2 Rule Group                                      | Implement georestriction rules for web traffic. Configuration: Block requests from Japan (JP).       |
| WebACL                  | WAFv2 Web Access Control List                        | Define access control rules for CloudFront. Configuration: Reference the RuleGroup for georestriction. |
| LoggingConfiguration    | WAFv2 Logging Configuration                          | Configure logging for the WebACL. Configuration: Log to the AccessLogBucket.                           |
| Bucket                  | S3 Bucket                                             | Host web content securely. Configuration: Private access control.                                           |
| OAI                     | CloudFront Origin Access Identity                    | Control access to S3 content from CloudFront.                                                           |
| Distribution            | CloudFront Distribution                              | Distribute web content globally. Configuration: Uses the defined Bucket and WebACL.                        |
| BucketPolicy            | S3 Bucket Policy                                      | Grant CloudFront access to the S3 Bucket. Configuration: Grants read access to CloudFront.                |
| Function                | Lambda Function                                       | Automate S3 object creation/deletion. Configuration: Creates 'index.html' in the S3 Bucket.               |
| FunctionRole            | IAM Role for Lambda                                   | Define permissions for the Lambda function. Configuration: Grants necessary S3 permissions.               |
| CustomResource          | Custom CloudFormation Resource                       | Trigger the Lambda function during CloudFormation stack operations.                                      |
| GlueDatabase            | AWS Glue Database                                     | Store metadata about Athena tables. Configuration: Uses the specified AthenaDB.                           |
| TransactionsTable       | AWS Glue Table                                        | Define a table for Athena to analyze. Configuration: Includes parameters and storage location.            |

### Outputs
CloudFrontURL: The URL of the CloudFront distribution.

## Conclusion
This CloudFormation template provides a comprehensive infrastructure for securing, logging, and analyzing web traffic. 
By leveraging AWS services such as WAF, CloudFront, S3, Lambda, and Glue, you can create a scalable and secure environment for your web applications. 
Feel free to customize the template to fit your specific requirements and enhance the security and analysis capabilities of your web infrastructure on AWS.
