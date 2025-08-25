CloudLaunch – AltSchool Cloud Engineering Assessment (Semester 3, Month 1)

📌 Project Overview
**Product Concept: CloudLaunch**

**You are working on a lightweight product called CloudLaunch, a platform that showcases a basic company website and stores some internal private documents. You are required to deploy it using AWS core services. This exercise demonstrates your understanding of AWS fundamentals including S3, IAM, and VPCs.**

## **Task 1: Static Website Hosting Using S3 + IAM User with Limited Permissions**
**Objective: Host a public website and manage private file storage using Amazon S3, and control access via IAM. The website should be a basic webpage for the CloudLauch project. Its content is not important, so much effort is not required.**

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

### *S3 Website URL:*
[s3-website url](http://cloud-launch-site-bucket-altsch.s3-website.eu-north-1.amazonaws.com/)

## *The private access bucket, has the following*
- Cloudlaunch-private-bucket
- Not publicly accessible.
- A designated IAM user (created by you) should have permissions to GetObject and PutObject only (no Delete).

## *The visible only access bucket, has the following*
- Cloudlaunch-visible-only-bucket
- Not publicly accessible.
- The IAM user should be able to list this bucket (see it in S3 list) but not access its contents.