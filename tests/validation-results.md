# CloudServe Validation Results

## Overview

This document summarises the main validation and failure tests completed for the CloudServe AWS Console implementation.

## 1. Application Health

**Endpoint:** `/health`  
**Expected result:** HTTP 200  
**Observed result:** Passed

The endpoint confirmed that Flask and Gunicorn were running and responding correctly on port `8000`.

## 2. Database Health

**Endpoint:** `/db-health`  
**Expected result:** HTTP 200 with a successful database connection  
**Observed result:** Passed

The test confirmed:

- access to AWS Secrets Manager;
- private connectivity to Amazon RDS;
- PostgreSQL authentication;
- TLS connectivity; and
- successful execution of a simple SQL query.

## 3. Target Group Health

**Expected result:** Two healthy targets  
**Observed result:** Passed

Both EC2 instances registered successfully with the Application Load Balancer target group and remained healthy.

## 4. Auto Scaling Self-Healing

**Test:** Terminate one active EC2 instance  
**Expected result:** The Auto Scaling Group launches a replacement instance  
**Observed result:** Passed

The terminated instance was replaced automatically. The new instance became `InService` and `Healthy`, restoring the desired capacity of two instances.

## 5. RDS Multi-AZ Failover

**Test:** Initiate a planned RDS failover  
**Expected result:** The standby becomes the new primary and application connectivity is restored  
**Observed result:** Passed

The failover completed successfully and the application continued using the same RDS endpoint.

## 6. HTTPS and Redirect

**Tests:**

- Open the custom domain using HTTPS.
- Open the domain using HTTP.

**Expected results:**

- HTTPS loads successfully using the ACM certificate.
- HTTP redirects to HTTPS.

**Observed result:** Passed

## 7. Monitoring and Notifications

**Tests:**

- Verify CloudWatch dashboard metrics.
- Confirm CloudWatch alarm actions.
- Test SNS email delivery.

**Observed result:** Passed

The configured alarms reached the expected state and the SNS email subscription was confirmed.

## 8. Security Validation

The following controls were validated:

- No public IP addresses on EC2 application instances.
- Amazon RDS is not publicly accessible.
- No SSH inbound rule.
- Systems Manager Session Manager is used for administration.
- ALB Security Group can reach the App Security Group only on TCP `8000`.
- App Security Group can reach the DB Security Group only on PostgreSQL TCP `5432`.

## Final Result

The CloudServe Console Version 1 architecture passed its core functional, security, availability, monitoring and recovery validation checks.
