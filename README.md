

## Project Title: Implementing File Storage with EFS

### Overview
This project demonstrates how to set up a highly available, distributed file system using AWS Elastic File Service (EFS), which provides fully managed shared file storage for Linux-based workloads running in AWS. We'll walk through creating an EFS file system, mounting it to EC2 instances, and verifying file sharing between them.

#### Architecture Diagram
![Amazon EFS architecture](architecture_diagram.png)

#### Pre-requisites
* An active AWS account
* Basic knowledge of AWS services like EFS, VPC, and EC2
* Familiarity with CLI tools such as `aws`, `ssh`, etc.

#### Step-by-step Guide

1. **Create a file system in EFS**
   * Go to the EFS Console, and then choose "Create file system" under "File systems".
   * Customize options according to requirements:
     ```yaml
     Name: <your-choice>
     Availability & Durability: Regional
     Performance Mode: General purpose
     Throughput Mode: Bursting
     Performance Settings: Enhanced
     ```
   * Select the existing Default VPC and delete the predefined security groups. Then, create a new security group allowing ports 22 (SSH) and 2049 (NFS) from anywhere.
   * Leave other configurations at their defaults and proceed to Review and Create the file system.

2. **Prepare EC2 instances**
   * Launch two EC2 instances within separate subnets but with the same security group. For example, select Subnet-1a and Subnet-1b while keeping the security group unchanged.
   * Add a shared file system during configuration by selecting the earlier created EFS file system and copy down the provided mount points for later usage.
   * Complete launching steps for each instance.

3. **Verify file sharing**
   * Connect to the first instance via EC2 Instance Connect. Verify if the EFS file system is mounted correctly with the following command:
     ```bash
     ls /mnt/efs/fs1/
     ```
   * Perform actions as root user:
     ```ruby
     sudo su
     ```
   * Create a test file:
     ```perl
     echo 'Hello World!' > /mnt/efs/fs1/hello.txt
     ```
   * Validate the contents of the file:
     ```bash
     cat /mnt/efs/fs1/hello.txt
     ```
   * Repeat these steps for the second instance after connecting via EC2 Instance Connect. Confirm that the content remains consistent with the previous instance, indicating proper file sharing over EFS.

#### Summary
In conclusion, implementing Amazon EFS enables users to establish a scalable, reliable, and high-throughput network file system accessible throughout various AWS regions and availability zones. In addition, using EFS facilitates easy maintenance without sacrificing performance when compared to traditional NFS solutions. Overall, this approach offers efficient management, cost savings, and improved disaster recovery capabilities due to built-in redundancy features.
