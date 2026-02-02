# aws-ec2-apache-s3-webapp
# Dynamic Apache Web Application on EC2 with Static Media Hosting on Amazon S3

Building a production-ready web application where HTML/PHP pages are served by Apache Web Server on EC2, while static assets like images and videos are stored in Amazon S3.EC2 accesses S3 using IAM Role for secure, keyless authentication, following AWS best practices for hybrid architecture design.

# Architecture & Learning Objectives

A dynamic web application combining the best of both worlds: EC2 
for compute-intensive dynamic content generation and S3 for 
efficient, scalable media delivery. This separation of concerns 
follows enterprise-grade architectural patterns used in 
production environments.
Apache + PHP dynamic pages on EC2
Images and videos hosted on S3
Secure IAM Role-based access
No hardcoded credentials anywhere
Optimized performance and cost

# Foundation: Storage & Security Setup
# 1.Create S3 Bucket for Media
Navigate to S3 and create a bucket named media-assets-hybrid- <unique-id> in the same region as your EC2 instance. Uncheck "Block all public access" to allow browserdirect media loading. This bucket will store all images and videos separately from EC2 for optimal scalability.

# 2.Upload Static Media Files
Upload your media assets: banner.jpg, logo.png, and sample.mp4. Organize them in folders: /images/ and /videos/. These files will be referenced by your EC2 website but served directly from S3 to browsers, reducing EC2 load and improving performance.

# 3.Configure S3 Bucket Policy
Apply a bucket policy allowing public read access to objects. This enables browsers to fetch images and videos directly from S3 without routing through EC2. The policy grants s3:GetObjectpermission to all principals for your bucket resources.

# 4.Create IAM Role for EC2
Create an IAM role named EC2-S3-MediaAccess with the AmazonS3ReadOnlyAccess policy attached. This role enables EC2 to securely access S3 without storing credentials. The role uses temporary credentials via instance metadata, following security best practices.

# EC2 Apache Stack & Dynamic Application

1.Launch EC2 Instance
Launch t2.micro with Amazon Linux 2023, attach the IAM role EC2-S3-MediaAccess, enable public IP, and configure security group for ports 22 (SSH) and 80 (HTTP).
2.Install Apache & PHP
Run: sudo yum install httpd php -y then enable and start Apache. PHP is required to execute dynamic logic; without it, browsers will download .php files instead of rendering them.
3.Create Dynamic Page
Create /var/www/html/index.php with PHP code that dynamically constructs S3 URLs. The browser fetches media directly from S3 while EC2 handles only the dynamic HTML generation.

# Installation Commands

* sudo yum update -y
* sudo yum install httpd php php-cli php-common php-json -y
* sudo systemctl enable httpd
* sudo systemctl start httpd

# Set Permissions
* sudo chown -R apache:apache /var/www/html
* sudo chmod -R 755 /var/www/html

# Restart Apache
* sudo systemctl restart httpd

# Final Architecture

User Browser
* |
* |---- HTTP ----> EC2 (Apache + Dynamic Web App)
* | |
* | |-- IAM Role -->
* | |-- Fetch Images & Videos -->
* |
* |---- HTTPS --------> S3 Bucket (Images / Videos)



