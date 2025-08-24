# AltSchool Cloud Engineering – Semester 3 Month 1 Assessment: CloudLaunch

## Overview
This repository contains the deliverables for the AltSchool of Cloud Engineering Tinyuka 2024 Third Semester Month 1 Assessment. The project involves deploying a lightweight product called CloudLaunch using AWS core services (S3, IAM, VPC). The assessment is divided into two tasks: hosting a static website on S3 with restricted IAM access and designing a secure VPC network layout.

## Task 1: Static Website Hosting Using S3 + IAM

### S3 Buckets
- cloudlaunch-site-bucket-jae25:
  - Hosts a static website (index.html).
  - Static website hosting enabled.
  - Publicly accessible (read-only) via bucket policy.
  - Website URL: http://cloudlaunch-site-bucket-jae25.s3-website-eu-west-1.amazonaws.com
  - (Bonus) CloudFront URL: https://d2as9d9iaudjuj.cloudfront.net
- cloudlaunch-private-bucket-jae25:
  - Private, accessible only by cloudlaunch-user with GetObject and PutObject permissions.
- cloudlaunch-visible-only-bucket-jae25:
  - Private, cloudlaunch-user can list the bucket but not access contents.

### IAM User
- User: cloudlaunch-user
- Permissions:
  - List all three buckets.
  - GetObject and PutObject on cloudlaunch-private-bucket.
  - GetObject on cloudlaunch-site-bucket.
  - No DeleteObject permissions.
  - No content access to cloudlaunch-visible-only-bucket.
- Policy: See CloudLaunchS3Policy.json in this repository.
- Console URL: https://227410687635.signin.aws.amazon.com/console
- Account ID: 227410687635
- Account Alias: cloudlaunch-user
- Credentials: Temporary password provided (user must change on first login).

### Setup Steps
1. Created three S3 buckets in eu-west-1 with specified configurations.
2. Uploaded index.html to cloudlaunch-site-bucket and set public read access via bucket policy.
3. Created cloudlaunch-user with a custom IAM policy (CloudLaunchS3Policy.json).
4. (Bonus) Configured CloudFront distribution for HTTPS and global caching.

## Task 2: VPC Design for CloudLaunch Environment

### VPC Configuration
- VPC Name: cloudlaunch-vpc
- CIDR Block: 10.0.0.0/16

### Subnets
- Public Subnet: cloudlaunch-public-subnet (10.0.1.0/24)
- Application Subnet: cloudlaunch-app-subnet (10.0.2.0/24)
- Database Subnet: cloudlaunch-db-subnet (10.0.3.0/28)

### Internet Gateway
- Name: cloudlaunch-igw
- Attached to cloudlaunch-vpc.

### Route Tables
- Public Route Table: cloudlaunch-public-rt
  - Associated with cloudlaunch-public-subnet.
  - Route: 0.0.0.0/0 → cloudlaunch-igw.
- Application Route Table: cloudlaunch-app-rt
  - Associated with cloudlaunch-app-subnet.
  - No internet route (private).
- Database Route Table: cloudlaunch-db-rt
  - Associated with cloudlaunch-db-subnet.
  - No internet route (private).

### Security Groups
- cloudlaunch-app-sg:
  - Allows HTTP (port 80) from VPC CIDR (10.0.0.0/16).
- cloudlaunch-db-sg:
  - Allows MySQL (port 3306) from Application Subnet (10.0.2.0/24).

### IAM Permissions
- cloudlaunch-user has read-only access to VPC components via CloudLaunchVPCReadOnly.json.

## Setup Steps
1. Created cloudlaunch-vpc with CIDR 10.0.0.0/16.
2. Configured subnets, internet gateway, route tables, and security groups as specified.
3. Attached read-only VPC permissions to cloudlaunch-user.

## Files in Repository
- index.html: Sample static website file.
- CloudLaunchS3Policy.json: IAM policy for S3 access.
- CloudLaunchVPCReadOnly.json: IAM policy for VPC read-only access.

## Notes
- All resources are within the AWS Free Tier.
- Best practices for access control and resource naming followed.
- CloudFront integration completed for bonus points.
