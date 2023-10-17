

# Launching an AWS EC2 Instance from the Command Line

This guide explains how to launch an Amazon Elastic Compute Cloud (EC2) instance on AWS using the AWS Command Line Interface (CLI). EC2 instances are virtual servers in the cloud that you can use to run applications and services.

## Prerequisites

Before you begin, make sure you have the following prerequisites:

1. **AWS CLI Installed**: Ensure that you have the AWS CLI installed on your local machine. If not, you can download and install it from the [AWS CLI official website](https://aws.amazon.com/cli/).

2. **AWS Account**: You need an AWS account with appropriate permissions to create and manage EC2 instances. Ensure you have your AWS access key ID and secret access key ready.

## Launching an EC2 Instance

Follow these steps to launch an EC2 instance from the command line:

### Step 1: Configure AWS CLI

If you haven't configured your AWS CLI, run the following command and provide your AWS access key ID, secret access key, default region, and output format:

```bash
aws configure
```

### Step 2: Create a Key Pair for Your Instance

Key pairs are an essential component of securing your AWS resources, particularly EC2 instances. Use the following command to create a key pair:

```bash
aws ec2 create-key-pair --key-name mykeypair --query 'KeyMaterial' --output text > mykeypair.pem
```

### Step 3: Choose an Amazon Machine Image (AMI)

An AMI is a pre-configured template that contains the software configuration needed to launch an instance. Choose an AMI that suits your requirements. For example, you can use the Amazon Linux 2 AMI:

```bash
ami_id=$(aws ec2 describe-images \
  --filters "Name=name,Values=Amazon Linux 2 AMI (HVM), SSD Volume Type" \
  --query "Images[0].ImageId" \
  --output text)
```

### Step 4: Create a Security Group

In AWS, a security group is a fundamental component of network security that acts as a virtual firewall for your Amazon Elastic Compute Cloud (EC2) instances and other AWS resources. Use this command to create a security group:

```bash
aws ec2 create-security-group --group-name MySecurityGroup --description "My security group"
```

### Step 5: Allow SSH Port

Allowing SSH access when launching an EC2 instance in AWS is essential for effective server administration, secure remote access, troubleshooting, and initial setup. However, it should be managed securely by implementing best practices to protect your EC2 instances from unauthorized access and potential security threats. Use the following command to allow SSH access:

```bash
aws ec2 authorize-security-group-ingress --group-name MySecurityGroup --protocol tcp --port 22 --cidr 0.0.0.0/0
```

### Step 6: Launch the Instance

Use the `aws ec2 run-instances` command to launch an EC2 instance. Customize the command with your desired instance type, security group, key pair, and any additional configurations:

```bash
aws ec2 run-instances \
  --image-id $ami_id \
  --instance-type t2.micro \
  --security-groups your-security-group \
  --key-name your-key-pair-name \
  --count 1
```

### Step 7: Access Your EC2 Instance

Once the instance is launched, you can access it via SSH or RDP depending on the operating system. Use the public IP or DNS name of your instance:


# For Linux instances

```bash
ssh -i your-key-pair.pem ec2-user@your-instance-public-ip
```

# For Windows instances
# Use an RDP client and the public IP of your instance


### Step 8: Terminate Your Instance (Optional)

Remember to terminate your EC2 instance when you're done to avoid ongoing charges:

```bash
aws ec2 terminate-instances --instance-ids your-instance-id
```

## Conclusion

You have successfully launched an EC2 instance on AWS using the AWS CLI. Make sure to monitor and manage your instances as needed, and always follow best practices for security and cost optimization.

For more information on AWS EC2 and its features, please refer to the [AWS EC2 documentation](https://docs.aws.amazon.com/ec2/). For virtual/video instruction, you can watch this YouTube video on [How to set up the AWS CLI to Programmatically Create EC2 Instances](https://youtu.be/FivN2qgKOPY?si=ZFmMwfg-MGHwdFiJ).


