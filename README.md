# AWS-Project--Secure-EC2-Instance-Management-with-Systems-Manager


## Problem Statement

**Case Study:** Operating EC2 instances securely through Systems Manager

One of the leading organizations offering media hosting platforms faces several challenges in maintaining its EC2 instances, which run in private subnets across multiple availability zones. The cases, which run Amazon Linux AMI and various software tools, require a robust solution to address the following issues:

1. **Resource Monitoring:** Need to monitor memory, CPU, and disk utilization, and take necessary measures if utilization exceeds or drops below certain levels.
2. **Update Downtime:** Instances face downtime during updates, necessitating measures to minimize or avoid downtime.
3. **Patching:** Ensure all operating system patches are installed periodically.
4. **Secure Access:** Implement a secure method for external connections to update application code on instances.
5. **Command Execution:** Ability to execute commands on all or a subset of instances.
6. **Compliance:** Ensure EC2 instances are managed instances to follow compliance requirements.

The objective is to develop a solution that addresses all the aforementioned challenges, ensuring security, compliance, and minimal downtime for the media hosting platform.

## Solution Overview

The solution involves creating a secure and manageable architecture using AWS resources such as VPC, EC2, IAM Roles, CloudWatch, and Systems Manager.

### Architecture Diagram

![image](https://github.com/Vsethi44/AWS-Project--Secure-EC2-Instance-Management-with-Systems-Manager/assets/151629020/48a3739b-327e-4f3a-8f12-2768dfe7f47a)



## Tasks and Implementation

### Task 1: Create VPC

**Purpose:** Establish a Virtual Private Cloud (VPC) to host EC2 instances securely.

Steps:
1. Create a VPC with the necessary CIDR blocks.
2. Create 2 private subnets and 2 public subnets.
3. Create 1 NAT Gateway per public subnet.
4. Configure route tables accordingly.

### Task 2: Create EC2 Instances

**Purpose:** Deploy EC2 instances running Amazon Linux 2 for the media hosting application.

Steps:
1. Launch two EC2 instances with Amazon Linux 2 AMI.
2. Choose instance type `t2.micro` and proceed without a key pair.
3. Select the VPC and private subnets created earlier.
4. Configure security groups to allow HTTP, HTTPS, and SSH traffic for VPC CIDR.
5. Create and attach an IAM role with `AmazonSSMFullAccess` and `AmazonSSMManagedInstanceCore` permissions.

### Task 3: Create IAM Role

**Purpose:** Define an IAM role to grant necessary permissions to EC2 instances.

Steps:
1. Create a role with the following policies:
   - `AmazonSSMFullAccess`
   - `AmazonSSMManagedInstanceCore`
   - `CloudWatchFullAccess`
   - `CloudWatchAgentAdminPolicy`
2. Name the role `SSMRole`.

### Task 4: Create Additional EC2 Instance

**Purpose:** Increase fault tolerance and redundancy.

Steps:
1. Launch another EC2 instance in a different availability zone.
2. Use the same VPC, private subnet, and IAM role (`SSMRole`).

### Task 5: Create CloudWatch Alarm

**Purpose:** Monitor CPU utilization to ensure optimal performance.

Steps:
1. Use AWS CloudWatch to create alarms for CPU utilization.
2. Set thresholds and notifications for CPU utilization exceeding 70%.

### Task 6: Monitor Memory Utilization

**Purpose:** Monitor memory utilization of EC2 instances.

Steps:
1. Create VPC endpoints for `com.amazonaws.us-east-1.ssm` and `com.amazonaws.us-east-1.ssmmessages`.
2. Use Systems Manager to install and configure the CloudWatch Agent.

### Task 7: Parameter Store for Agent Configuration

**Purpose:** Centralize and manage configuration settings for CloudWatch Agent.

Steps:
1. Create a parameter in Systems Manager Parameter Store.

### Task 8: Patch Manager

**Purpose:** Automate patching of EC2 instances to ensure security and compliance.

Steps:
1. Use Systems Manager Patch Manager to create patch policies.

### Task 9: AWS Config

**Purpose:** Ensure all EC2 instances are managed by Systems Manager as per compliance requirements.

Steps:
1. Use AWS Config to check if instances are managed instances.
2. Apply the `ec2-instance-managed-by-systems-manager` rule.

## Conclusion

With this architecture, we can effectively monitor and maintain EC2 instances, ensuring security, compliance, and minimal downtime for the media hosting platform.

