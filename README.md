CloudLaunch – AltSchool Cloud Engineering Assessment (Semester 3, Month 1)

📌 Project Overview
**Product Concept: CloudLaunch**

**You are working on a lightweight product called CloudLaunch, a platform that showcases a basic company website and stores some internal private documents. You are required to deploy it using AWS core services. This exercise demonstrates your understanding of AWS fundamentals including S3, IAM, and VPCs.**

## **Task 1: Static Website Hosting Using S3 + IAM User with Limited Permissions**
**Objective: Host a public website and manage private file storage using Amazon S3, and control access via IAM. The website should be a basic webpage for the CloudLauch project. Its content is not important, so much effort is not required.**

---

## **S3 Static Website Hosting**

## *The public access bucket, has the following*
- Cloudlaunch-site-bucket
- Hosts a simple static website (HTML/CSS/JS).
- Enable static website hosting.
- Publicly accessible (read-only for anonymous users).

- Created an S3 bucket named cloudlaunch-bucket.

- Configured bucket policy to allow public access for website hosting.

- Uploaded static website files (index.html, error.html).

- Enabled static website hosting under bucket properties.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::cloudlaunch-site-bucket/*"
    }
  ]
}
```
---

### *S3 Website URL:*
[s3-website url](http://cloud-launch-site-bucket-altsch.s3-website.eu-north-1.amazonaws.com/)

## *The private access bucket, has the following*
- Cloudlaunch-private-bucket
- Not publicly accessible.
- A designated IAM user (created by you) should have permissions to GetObject and PutObject only (no Delete).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::cloudlaunch-private-bucket",
        "arn:aws:s3:::cloudlaunch-private-bucket/*"
      ]
    }
  ]
}
```
---

## *The visible only access bucket, has the following*
- Cloudlaunch-visible-only-bucket
- Not publicly accessible.
- The IAM user should be able to list this bucket (see it in S3 list) but not access its contents.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowListBucket",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::cloudlaunch-visible-only-bucket"
    }
  ]
}
```
---

###### **Task 2: VPC Design for CloudLaunch Environment**

** The objective of Task 2 is to design and implement a secure Virtual Private Cloud (VPC) in AWS that demonstrates understanding of network segmentation, routing, and access control.
This includes creating public and private subnets, associating route tables, and configuring Internet access only for resources that need it. **


# CloudLaunch - Task 2: VPC Design

This task demonstrates the design of a secure Virtual Private Cloud (VPC) in AWS to host the CloudLaunch platform. The design includes subnets, route tables, an Internet Gateway, Security Groups, and IAM policies for restricted access.

---

## VPC
- **Name:** `cloudlaunch-vpc`
- **CIDR:** `10.0.0.0/16`

### Screenshot
![VPC Screenshot](./TASK%202/screenshots/Vpc.png)

---

## Subnets
1. **Public Subnet**
   - Name: `cloudlaunch-public-subnet`
   - CIDR: `10.0.1.0/24`
   - Used for resources needing internet access.
   
   ![Public Subnet](/TASK%202/screenshots/public-subnet.png)

2. **Application Subnet**
   - Name: `cloudlaunch-application-subnet`
   - CIDR: `10.0.2.0/24`
   - Hosts application servers (private).
   
   ![Application Subnet](/TASK%202/screenshots/app-subnet.png)

3. **Database Subnet**
   - Name: `cloudlaunch-database-subnet`
   - CIDR: `10.0.3.0/24`
   - Hosts the database layer (private).
   
   ![Database Subnet](/TASK%202/screenshots/db-subnet.png)

---

## Route Tables
1. **Public Route Table**
   - Associated with the public subnet.
   - Routes internet traffic through the Internet Gateway.
   
   ![Public Route Table](/TASK%202/screenshots/public-rt.png)

2. **Private Route Table**
   - Associated with application and database subnets.
   - Routes traffic internally only.
   
   ![Application Route Table](/TASK%202/screenshots/app-rt.png)

   ![Database Route Table](/TASK%202/screenshots/database-rt.png)

---

## Internet Gateway
- **Name:** `cloudlaunch-igw`
- Provides internet access for resources in the public subnet.

![Internet Gateway](/TASK%202/screenshots/internet-gateway.png)

---

## Security Groups
1. **Application SG** (`cloudlaunch-app-sg`)
   - Inbound: Allow HTTP (80), HTTPS (443) from the internet.
   - Outbound: Allow all.

   ![Application SG](/TASK%202/screenshots/app-sg.png)

2. **Database SG** (`cloudlaunch-db-sg`)
   - Inbound: Allow MySQL (3306) only from `cloudlaunch-app-sg`.
   - Outbound: Allow all.

   ![Database SG](/TASK%202/screenshots/database-sg.png)

---

##  IAM Permissions:
*Your IAM user cloudlaunch-user should have read-only (list/view) access to the VPC alone and its components (subnets, route table, security groups).*

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeRouteTables",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeInternetGateways",
        "ec2:DescribeVpcEndpoints"
      ],
      "Resource": "*"
    }
  ]
}
```
This json policy above ensures cloudlaunch-user can only view (read-only) the VPC and its related components without being able to create or delete resources.

## Summary
This setup ensures:
- Public subnet for external resources.
- Private subnets for secure application and database layers.
- Route tables for controlled traffic flow.
- Security groups to enforce least privilege.
- IAM policy for controlled admin access.
