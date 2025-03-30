# AWS-Three-Tier-Web-Architecture-Workshop

## Description:
In this project, I'll be provisioning a 3-tier application (Web layer, Application layer, Data storage layer) on AWS using `Terraform` (Originally in the article was configured manually in AWS console).

### This architecture are popular pattern and can provide
1. scalability
2. availability
3. security

This design is focusing on seperate an application into three layers, with specific function and independent from other also spreading the application across multiple availability zones and seperate into three layers If any of availability zone is gones down, the application can automatically scale resources to another availibility zone with affecting the rest of the application

Each tier has its own security group (only allows just "necessary" in and out bound to perform specfic tasks).

# Recap The resource we need
**Stored our code**
1. S3 Bucket (ACLs Disabled)
2. IAM Role for S3 Bucket Attach policies: `AmazonSSMManagedInstanceCore`, `AmazonS3ReadOnlyAccess`
3. Upload our code into S3 Bucket

**VPC**
1. Create VPC with CIDR range 10.0.0.0/16
2. Create Subnet `Public-Web-Subnet-AZ-1` - `10.0.0.0/24` - `ap-southeast-1a`
3. Create Subnet `Public-Web-Subnet-AZ-2` - `10.0.1.0/24` - `ap-southeast-1b`
4. Create Subnet `Private-App-Subnet-AZ-1` - `10.0.2.0/24` - `ap-southeast-1a`
5. Create Subnet `Private-App-Subnet-AZ-2` - `10.0.3.0/24` - `ap-southeast-1b`
6. Create Subnet `Private-DB-Subnet-AZ-1` - `10.0.4.0/24` - `ap-southeast-1a`
7. Create Subnet `Private-DB-Subnet-AZ-2` - `10.0.5.0/24` - `ap-southeast-1b`
8. Create Internet Gateway and attach it to the VPC
9. Create NAT Gateway in the public subnet of AZ-1, AZ-2
10. Create Route Tables and associate them with subnets
11. Create Security Groups `internet-facing-lb-sg`, Allow: `HTTP`, Source: `Anywhere`
12. Create Security Groups `webtier-sg`, Allow: `HTTP,HTTP`, Source: `internet-facing-lb-sg, My IP` (This will allow traffic from your public facing load balancer to hit your instances)
13. Create Security Groups `internal-lb-sg`, Allow: `HTTP`, Source: `internet-facing-lb-sg`
14. Create Security Groups `private-instance-sg`, Allow: `Custom TCP, Custom TCP`, Port Range: `4000, 4000`, Source: `internal load balancer security group, My IP`
15. Create Security Groups `DB-sg`, Type: `MYSQL/Aurora`, Protocol: `TCP`, Port range: `3306`, Source: `private-instance-sg`

**Subnet Groups**
1. Create Subnet group `three-tier-db-subnet-group`, VPC: `vpc_from_previous_step`, Add subnets (Avaliablity Zones): `ap-southeast-1a, ap-southeast-1b`

**Database Deployment**
