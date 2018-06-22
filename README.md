# Update Cloudfront Security Groups

Triggered by an AWS SNS notification that their IP blocks have been updated, this function will automatically update your Security Groups with specific tags.

## Deploy

In order to create the SNS subscription this function needs to be deployed to us-east-1.

### Serverless Application Repository

The serverless application Repository only supports a subset of permissions. Currently it does not support the Security Group Permissions this function needs. Once the function is deployed you will need to update the Lambda execution role created with the following inline policy.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:RevokeSecurityGroupIngress"
            ],
            "Resource": "arn:aws:ec2:[region]:[account-id]:security-group/*"
        },
        {
            "Effect": "Allow",
            "Action": "ec2:DescribeSecurityGroups",
            "Resource": "*"
        },
        {
            "Action": [
                "logs:CreateLogGroup",
                 "logs:CreateLogStream",
                 "logs:PutLogEvents"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:*"
        }
    ]
}
```

## Usage

Tag your security groups you want updated with the following.

- Name: cloudfront_g and AutoUpdate: true and a Protocol tag with value http or https.
- Name: cloudfront_r and AutoUpdate: true and a Protocol tag with value http or https.

## Authors

- **Jeff Finley** - *Initial work* - [FiveTalent](https://fivetalent.com)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

- Lambda function borrowed from [aws-cloudfront-samples](https://github.com/aws-samples/aws-cloudfront-samples/tree/master/update_security_groups_lambda) and modified slightly
- Inspired by [this](https://aws.amazon.com/blogs/security/how-to-automatically-update-your-security-groups-for-amazon-cloudfront-and-aws-waf-by-using-aws-lambda/) blog post.

Made with ❤️ by Five Talent. Available on the [AWS Serverless Application Repository](https://aws.amazon.com/serverless)
