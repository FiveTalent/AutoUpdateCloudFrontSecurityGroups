# Update Cloudfront Security Groups

This is a lambda function borrowed from [aws-cloudfront-samples](https://github.com/aws-samples/aws-cloudfront-samples/tree/master/update_security_groups_lambda). Triggered by an AWS SNS notification that their IP blocks have been updated, this function will update your Security Groups based on some tags.

## Deploy

In order to create the SNS subscription this function needs to be deployed to us-east-1. However a region of us-west-2 is set in the function itself.

### Package

`aws s3api create-bucket --bucket bucket-name --region us-east-1`
`aws cloudformation package --template-file ./template.yaml --s3-bucket bucket-name --output-template-file packaged-template.yaml`

### Deploy function

`aws cloudformation deploy --template-file ./packaged-template.yaml --stack-name auto-update-cloudfront-security-groups --capabilities CAPABILITY_IAM --region us-east-1 --parameter-overrides SecurityGroupRegion=us-west-2`

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
