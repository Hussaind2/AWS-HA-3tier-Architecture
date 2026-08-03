# CloudServe Implementation Guide

## 1. Overview

CloudServe is a production-style customer order and support portal deployed on AWS using a highly available three-tier architecture.

The infrastructure was implemented manually through the AWS Management Console to build practical experience in networking, compute, database deployment, security, availability and troubleshooting.

The architecture was deployed in the `us-east-1` Region.

---

## 2. Architecture Summary

The solution separates the infrastructure into three tiers:

### Web Tier

An Application Load Balancer receives incoming traffic through public subnets and distributes requests across healthy application instances.

### Application Tier

The Python Flask application runs on Amazon EC2 instances located in private subnets. The instances are managed by an Auto Scaling Group and distributed across multiple Availability Zones.

### Database Tier

Amazon RDS for PostgreSQL is deployed using a Multi-AZ configuration in private database subnets. The database is accessible only from the application tier.

---

## 3. Cost and Account Preparation

Before deploying the infrastructure, account-level cost controls were configured.

The preparation included:

- Enabling multi-factor authentication.
- Configuring AWS IAM Identity Center.
- Creating AWS Budgets alerts at several spending thresholds.
- Selecting `us-east-1` as the deployment Region.
- Applying consistent project and environment tags.

The main tags used throughout the project were:

```text
Project     = CloudServe
Environment = Portfolio
ManagedBy   = Console
