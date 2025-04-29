# Custom VPC Creation on AWS

This guide walks you through the process of creating a custom Virtual Private Cloud (VPC) on AWS. You will create both public and private subnets, set up necessary networking components like Internet Gateway and NAT Gateway, and launch EC2 instances to test connectivity.

---

## 🧠 Learning Objectives

- Understand the structure of a custom VPC
- Create and configure public and private subnets
- Set up Internet Gateway and NAT Gateway
- Configure Network ACLs for enhanced security
- Deploy EC2 instances in different subnet tiers
- Learn basic routing and access management within AWS networking

---

## ✅ Prerequisites

- An active AWS account
- Basic understanding of AWS networking components
- IAM user with required permissions to create VPC, EC2, and related resources
- Familiarity with AWS Console and basic cloud terminology

---

## 🔐 Step 1: Logging In to the Amazon Web Services Console

1. Go to [AWS Console](https://console.aws.amazon.com/)
2. Login with your credentials
3. Choose the region where you want to create the VPC

---

## 🌐 Step 2: Creating a VPC

1. Navigate to **VPC Dashboard**
2. Click on **Create VPC**
3. Choose **VPC only**
4. Name your VPC (e.g., `CustomVPC`)
5. Provide IPv4 CIDR block (e.g., `10.0.0.0/16`)
6. Leave IPv6 as default (optional)
7. Choose Tenancy as **default**
8. Click **Create VPC**

---

## 🌉 Step 3: Creating a VPC Internet Gateway

1. Go to **Internet Gateways**
2. Click **Create internet gateway**
3. Name it (e.g., `CustomIGW`)
4. Attach the Internet Gateway to your VPC

---

## 🌍 Step 4: Creating a Public Subnet

1. Go to **Subnets**
2. Click **Create subnet**
3. Select the created VPC
4. Name it (e.g., `PublicSubnet`)
5. Specify an Availability Zone and CIDR (e.g., `10.0.1.0/24`)
6. Enable auto-assign public IP
7. Click **Create subnet**

---

## 🔐 Step 5: Creating a Bastion Host

1. Navigate to **EC2 > Launch Instance**
2. Name it `BastionHost`
3. Use Amazon Linux 2 or any preferred AMI
4. Launch into the Public Subnet
5. Assign a public IP
6. Select an appropriate key pair
7. Ensure Security Group allows SSH (port 22) access

---

## 🔒 Step 6: Creating a Private Subnet

1. Go to **Subnets > Create subnet**
2. Name it (e.g., `PrivateSubnet`)
3. Choose CIDR (e.g., `10.0.2.0/24`)
4. Select same VPC and another Availability Zone
5. Do **not** auto-assign public IP

---

## 🔐 Step 7: Creating a Network ACL for a Private Subnet

1. Go to **Network ACLs**
2. Click **Create Network ACL**
3. Associate it with your VPC
4. Name it (e.g., `PrivateNACL`)
5. Associate with the Private Subnet

---

## 🔁 Step 8: Adding Rules to a Private Network ACL

### Inbound Rules:
- Allow SSH (port 22) from Bastion Host CIDR
- Allow HTTP/HTTPS as per need

### Outbound Rules:
- Allow all traffic to the internet (0.0.0.0/0)

---

## 💻 Step 9: Launching an EC2 Instance on a Private Subnet

1. Navigate to **EC2 > Launch Instance**
2. Name it `PrivateInstance`
3. Use a compatible AMI
4. Launch into the Private Subnet
5. Use the same key pair
6. No public IP
7. SSH into it using Bastion Host as a jump server

---

## 🌐 Step 10: Launching a Network Address Translation (NAT) Gateway

1. Go to **NAT Gateways**
2. Click **Create NAT Gateway**
3. Select the Public Subnet
4. Allocate an Elastic IP
5. Attach NAT Gateway
6. Update the **Route Table** for Private Subnet:
   - Add route for `0.0.0.0/0` to NAT Gateway

---

## ✅ Summary

You have successfully created a custom VPC setup with:
- A VPC with public and private subnets
- An Internet Gateway for internet access
- A Bastion Host for secure SSH into private instances
- A NAT Gateway to enable internet access for private subnet instances
- Proper network ACLs and route tables for controlled traffic flow

This setup is ideal for deploying secure applications with a proper network architecture in AWS.

---

