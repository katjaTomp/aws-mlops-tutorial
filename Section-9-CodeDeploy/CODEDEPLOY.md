
# Installing the CodeDeploy agent on EC2
```YAML

- Create EC2 Instance with IAM Role attached
    - Assign the Policy `AmazonS3ReadOnlyAccess` to allow CodeDeploy agent to read the version from S3 Bucket
    - Add User Data as mentioned below
- Assign tags to EC2 Instances
- Launch the instance and run the command - `sudo service codedeploy-agent status` to validate - CodeDeploy Agent is not running in EC2 instance
- Create an Application in CodeDeploy
- Push app revision to S3 Bucket (create a S3 bucket with versioning if its not created) - see section - **deploy the files into S3** below
- Create a Service Role for CodeDeploy and assign Codedeploy policy
- Create CodeDeployment Group and assign IAM role created above
- Do necessary settings and create Code Deployment
- Run the Deployment
- Verify whether the website is working (Make sure to check the security group of ec2 instance)
```
# Manual Steps for CodeDeploy Agent
```
sudo yum update -y
sudo yum install -y ruby
sudo yum install -y wget
cd /home/ec2-user
wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status

#If error
sudo service codedeploy-agent start
sudo service codedeploy-agent status
```


# create a bucket and enable versioning
## https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html
## https://docs.aws.amazon.com/cli/latest/reference/s3api/put-bucket-versioning.html
```
aws s3 mb s3://aws-manifold-devops --region ap-south-1 
aws s3api put-bucket-versioning --bucket aws-manifold-devops --versioning-configuration Status=Enabled --region ap-south-1 
```

# deploy the files into S3
# https://docs.aws.amazon.com/cli/latest/reference/deploy/push.html
```
aws deploy push --application-name demo-app --s3-location s3://demo-app-aws-yt/demo-app/app.zip --ignore-hidden-files --region us-east-1 
```


# User data
```
#!bin/bash
sudo yum update -y
sudo yum install -y ruby wget
wget https://aws-codedeploy-eu-west-1.s3.eu-west-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status
```
