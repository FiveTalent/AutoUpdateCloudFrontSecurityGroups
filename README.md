# Update Cloudfront Security Groups

This is a lambda function borrowed from [aws-cloudfront-samples](https://github.com/aws-samples/aws-cloudfront-samples/tree/master/update_security_groups_lambda). Triggered by an AWS SNS notification that their IP blocks have been updated, this function will update your Security Groups based on some tags.

## Deploy

### Deploy function

```bash
serverless deploy
```

### Update IAM policy

There is a current issue with the policy in serverless.yml so we need to updated the policy manually.

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

### Subscribe to SNS notification

Currently serverless can not subscribe you to an sns topic in a different region.

```bash
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:806199016981:AmazonIpSpaceChanged --protocol lambda --notification-endpoint <Lambda ARN>
```

## Usage

Tag your security groups you want updated with the following.

- Name: cloudfront_g and AutoUpdate: true and a Protocol tag with value http or https.
- Name: cloudfront_r and AutoUpdate: true and a Protocol tag with value http or https.

## Todo

- [] Fix role permissions in yml file.
- [] Add subscribing to ARN - Current cross region issue.
