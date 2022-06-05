# CLI: Command Line Interface

Add user credentials locally using this command:
- $ `aws configure`
- Use the key and access for the user role created. 

If you are using multiple AWS accounts, you can add custom profiles with seperate credentials using this command:
- $ `aws configure --profile {my-other-aws-account}`
- if you you'd like to execute commands on a specific profile:
  - example: `aws s3 ls --profile {my-other-aws-account}`
- if you don't specify the aws profile, the commands will be executed to your **default** profile

#### AWS CLI on EC2
* IAM roles can be attached to EC2 instances
* IAM roles can come with a policy authorizing exactly what the EC2 instance should be able to do. This is the best practice. 
  - always give the minimum amount of access previlage. 
* EC2 Instances can then use these profiles automatically without any additional configurations
  - change the SSH permission: chmod 400 mykp.pem; ssh es2-user@xxx.xxx.xxx.xxx -i mykp.pem

#### CLI STS Decode Errors
- When you run API calls and they fail, you can get a long, encoded error message code 
- This error can be decoded using STS 
- run the command: `aws sts decode-authorization-message --encoded-message {encoded_message_code}`
- your IAM user must have the correct permissions to use this command by adding the `STS` service to your policy
- Time out error: retreive the objects above 1000 under s3 buckets - need multiple calls; could adjust the page size/number of data retreive per call to solve the error. 
  - page-size 100
  - max-items 100
 
