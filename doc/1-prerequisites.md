# Prerequisites

## Description

In this section we will create all the required resources that we need for the next steps:

 1. EC2 Key Pair
 2. Security Group
 3. IAM Role for EC2 Instances
 4. IAM Role for CodeBuild
 5. IAM Role for CodeDeploy
 
---
 
## Create an EC2 Key Pair

Let's create a SSH key to access our servers

 - Browse to "Key Pairs" (under Network & Security) in the EC2 portal

<kbd>
  <img src="/doc/images/01-01-keypair-01.png" width="800">
</kbd>

 - Then click to "Create key pair"

<kbd>
  <img src="/doc/images/01-01-keypair-02.png" width="800">
</kbd>

 - Create a new key with the following details:
   - Name: bluegreen-key
   - File format: pem

<kbd>
  <img src="/doc/images/01-01-keypair-03.png" width="800">
</kbd>

 - The key will be downloaded automatically, store it in a secure place (you will need it after)

---

## Create a Security Group

Now let's create a security group to allow traffic to our servers

 - Browse to "Security groups" (under Network & Security) in the EC2 portal

<kbd>
  <img src="/doc/images/01-02-securitygroup-01.png" width="800">
</kbd>

 - Then click to "Create security group"

<kbd>
  <img src="/doc/images/01-02-securitygroup-02.png" width="800">
</kbd>

 - Create a new key with the following details
   - Name: bluegreen-sg
   - Description: all open
   - VPC: <the-vpc-where-your-servers-will-run>
   - Inbound Rules:
     - Type: All traffic
     - Source: Anywhere
   - Outbound rules: <keep-default>

<kbd>
  <img src="/doc/images/01-02-securitygroup-03.png" width="800">
</kbd>

---

## Create an IAM Role for your EC2 Instances

Now we need to create an IAM role for your instances to grant access to S3

 - Browse to "Policies" in the IAM portal
 
<kbd>
  <img src="/doc/images/01-03-iamroleinstance-01.png" width="800">
</kbd>

 - Click on "Create policy"

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-02.png" width="800">
</kbd>

 - Click on the "JSON" tab, paste the below policy and click on "Review policy"

 ```
 {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
 ```

 <kbd>
  <img src="/doc/images/01-03-iamroleinstance-03.png" width="800">
</kbd>

 - Create the policy with the details below:
   - Name: bluegreen-ec2-policy
   - Description: allow access to s3 from ec2 instance
 
 <kbd>
  <img src="/doc/images/01-03-iamroleinstance-04.png" width="800">
</kbd>

 - Browse to "Roles" in the IAM portal

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-05.png" width="800">
</kbd>

 - Click on "Create role"

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-06.png" width="800">
</kbd>

 - Create a role with the following details:
   - Type of trusted entity: AWS service
   - Use case: EC2

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-07.png" width="800">
</kbd>

 - Then attach the policy created previously "bluegreen-ec2-policy" and click "Next:Tags"

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-08.png" width="800">
</kbd>

 - Add a tag (optional) and click "Next: Review"

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-09.png" width="800">
</kbd>

 - Finally create the role by click "Create role" (name it "bluegreen-ec2-role")

<kbd>
  <img src="/doc/images/01-03-iamroleinstance-10.png" width="800">
</kbd>

---

## Create an IAM Role for CodeBuild

 - Browse to "Policies" in the IAM portal

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-01.png" width="800">
</kbd>

 - Click on "Create policy"

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-02.png" width="800">
</kbd>

 - Click on the "JSON" tab, paste the below policy and click on "Review policy"

 ```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Resource": "*",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ]
        },
        {
            "Effect": "Allow",
            "Resource": "*",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "codebuild:CreateReportGroup",
                "codebuild:CreateReport",
                "codebuild:UpdateReport",
                "codebuild:BatchPutTestCases"
            ],
            "Resource": "*"
        }
    ]
}
 ```

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-03.png" width="800">
</kbd>

 - Create the policy with the details below:
   - Name: bluegreen-codebuild-policy
   - Description: allow required access for codebuild

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-04.png" width="800">
</kbd>

 - Browse to "Roles" in the IAM portal

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-05.png" width="800">
</kbd>

 - Now let's create another role for CodeBuild by click on "Create role"

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-06.png" width="800">
</kbd>

 - Create a role with the following details:
   - Type of trusted entity: AWS service
   - Use case: CodeBuild

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-07.png" width="800">
</kbd>

 - Then attach the policy created previously "bluegreen-codebuild-policy" and click "Next:Tags"

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-08.png" width="800">
</kbd>

 - Add a tag (optional) and click "Next: Review"

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-09.png" width="800">
</kbd>

 - Finally create the role by click "Create role" (name it "bluegreen-codebuild-role")

<kbd>
  <img src="/doc/images/01-04-iamrolecodebuild-10.png" width="800">
</kbd>

---

## Create an IAM Role for CodeDeploy

 - Now let's create another role for CodeDeploy by click on "Create role"

<kbd>
  <img src="/doc/images/01-05-iamrolecodedeploy-01.png" width="800">
</kbd>

 - Create a role with the following details:
   - Type of trusted entity: AWS service
   - Use case: CodeDeploy

<kbd>
  <img src="/doc/images/01-05-iamrolecodedeploy-02.png" width="800">
</kbd>

 - Then keep the policy called "AWSCodeDeployRole" and click "Next:Tags"

<kbd>
  <img src="/doc/images/01-05-iamrolecodedeploy-03.png" width="800">
</kbd>

 - Add a tag (optional) and click "Next: Review"

<kbd>
  <img src="/doc/images/01-05-iamrolecodedeploy-04.png" width="800">
</kbd>

 - Finally create the role by click "Create role" (name it "bluegreen-codedeploy-role")

<kbd>
  <img src="/doc/images/01-05-iamrolecodedeploy-05.png" width="800">
</kbd>
