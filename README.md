# AWS-Three-Tier-Web-Architecture-Workshop

## Description:
In this project, I'll be provisioning a 3-tier application (Web layer, Application layer, Data storage layer) on AWS using `Terraform` (Originally in the article was configured manually in the AWS console).

# Recap: The resource we need
**Stored our code**
1. S3 Bucket (ACLs Disabled)
2. IAM Role for S3 Bucket Attach policies: `AmazonSSMManagedInstanceCore`, `AmazonS3ReadOnlyAccess`
3. Upload our code into the  S3 Bucket

**VPC**
1. Create a VPC with a CIDR range of 10.0.0.0/16
2. Create Subnet `Public-Web-Subnet-AZ-1` - `10.0.0.0/24` - `ap-southeast-1a`
3. Create Subnet `Public-Web-Subnet-AZ-2` - `10.0.1.0/24` - `ap-southeast-1b`
4. Create Subnet `Private-App-Subnet-AZ-1` - `10.0.2.0/24` - `ap-southeast-1a`
5. Create Subnet `Private-App-Subnet-AZ-2` - `10.0.3.0/24` - `ap-southeast-1b`
6. Create Subnet `Private-DB-Subnet-AZ-1` - `10.0.4.0/24` - `ap-southeast-1a`
7. Create Subnet `Private-DB-Subnet-AZ-2` - `10.0.5.0/24` - `ap-southeast-1b`
8. Create an Internet Gateway and attach it to the VPC
9. Create a NAT Gateway in the public subnet of AZ-1, AZ-2
10. Create Route Tables and associate them with subnets
11. Create Security Groups `internet-facing-lb-sg`, Allow: `HTTP`, Source: `Anywhere`
12. Create Security Groups `webtier-sg`, Allow: `HTTP, HTTP`, Source: `internet-facing-lb-sg, My IP` (This will allow traffic from your public-facing load balancer to hit your instances)
13. Create Security Groups `internal-lb-sg`, Allow: `HTTP`, Source: `internet-facing-lb-sg`
14. Create Security Groups `private-instance-sg`, Allow: `Custom TCP, Custom TCP`, Port Range: `4000, 4000`, Source: `internal load balancer security group, My IP`
15. Create Security Groups `DB-sg`, Type: `MYSQL/Aurora`, Protocol: `TCP`, Port range: `3306`, Source: `private-instance-sg`

**Subnet Groups**
under construction 🚧🚧

**Database Deployment**
🚧🚧
