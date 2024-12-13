# XYZ Corporation Web Application Project

## Introduction
This project is for setting up a **web-based application** infrastructure on **AWS**. It involves creating a **three-tier architecture** with a **Web Tier**, **Application Tier**, and **Database Tier**. The project allows the development team to test their code without requiring system admins for provisioning and configuring resources.

## Overview of the Project
This project involves setting up:
- **Web Tier**: A web server to test the application code.
- **Application Tier**: A private instance that communicates with the web server.
- **Database Tier**: An RDS MySQL instance to store application data.

## Technologies Used
- **AWS**: EC2, VPC, Route 53, RDS, IAM
- **MySQL**: Database for storing application data
- **Security Groups**: To control traffic between instances
- **CloudFormation**: To automate infrastructure setup

## Architecture

- **Web Tier**:
  - EC2 instance in a **public subnet**.
  - Allows **HTTP and SSH** traffic from the internet.
  
- **Application Tier**:
  - EC2 instance in a **private subnet**.
  - Allows **SSH** traffic only from the Web Tier instance.
  
- **DB Tier**:
  - RDS MySQL instance in a **private subnet**.
  - Allows connection on **port 3306** only from the Application Tier.

- **Route 53**:
  - Setup to direct traffic to the EC2 instance in the Web Tier.

## Setup Instructions

### 1. **Create the Infrastructure**
To set up the AWS infrastructure for the three-tier architecture:

1. **Create a VPC** with public and private subnets.
2. **Launch EC2 instances** for the Web Tier and Application Tier.
3. **Launch an RDS MySQL instance** for the DB Tier in the private subnet.
4. **Setup security groups** to allow traffic between the tiers:
   - Web Tier SG: Allow HTTP and SSH from the internet.
   - Application Tier SG: Allow SSH from the Web Tier.
   - DB Tier SG: Allow MySQL traffic (port 3306) from the Application Tier.

### 2. **Enable Deletion Protection for the RDS Instance**
To prevent the accidental deletion of the RDS instance:
- When creating the RDS instance, enable **Deletion Protection**.

### 3. **CloudFormation Stack**
For automating the setup, you can use a **CloudFormation** stack to launch the infrastructure. This will help you:
- Automatically provision the EC2 instances and RDS.
- Set up security groups, VPC, and subnets.

### 4. **Testing the Code**
The development team can now test the application by:
- Accessing the **Web Tier EC2 instance** via SSH or HTTP.
- Interacting with the **Application Tier** and **DB Tier** through the private network.

### 5. **Route 53 Setup**
Configure **Route 53** to point to the EC2 instance in the Web Tier:
- Create a hosted zone.
- Add a record to route traffic to the EC2 instance’s public IP.

## How Developers Can Test the Application
1. After the infrastructure is set up, the development team can access the Web Tier instance via SSH or HTTP.
2. They can deploy their code and perform tests on the application.
3. Once testing is completed, the team can delete the CloudFormation stack.

### 6. **Preserve RDS Database**
To prevent the deletion of the RDS database when the stack is deleted:
- Ensure that **deletion protection** is enabled for the RDS instance.
- You may also want to create an **RDS Snapshot** before deleting the stack to preserve data.

## IAM Roles and Policies for Developers
To allow the development team to manage the infrastructure and test the application:
1. Create an **IAM Group** for developers.
2. Attach policies like:
   - **AmazonEC2FullAccess** for EC2 instances.
   - **AmazonRDSFullAccess** for RDS management.
3. Enable the **DeleteStackProtection** IAM policy to prevent deletion of RDS instances.

## Conclusion
This setup ensures that the development team can test their code in a secure and isolated environment without needing system admins for infrastructure provisioning. Additionally, the RDS instances are protected from deletion, ensuring the database persists after testing.

---

## License
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

