# CloudServe – Highly Available 3-Tier Architecture on AWS

CloudServe is a production-style customer order and support portal deployed using a highly available three-tier architecture on AWS.

## Project Objectives

- Design a secure and highly available AWS architecture.
- Separate the web, application and database tiers.
- Deploy a Python Flask application on Amazon EC2.
- Use Amazon RDS for PostgreSQL with Multi-AZ deployment.
- Distribute traffic using an Application Load Balancer.
- Implement monitoring, security controls and failure testing.
- Rebuild the infrastructure later using Terraform.

## Architecture Overview

The project will use three main tiers:

1. **Web Tier** – Application Load Balancer in public subnets.
2. **Application Tier** – Flask application hosted on EC2 instances in private subnets.
3. **Database Tier** – Amazon RDS for PostgreSQL in isolated private database subnets.

## AWS Region

`us-east-1`

## Project Status

✅ Completed – The architecture has been designed, deployed, tested and documented.
