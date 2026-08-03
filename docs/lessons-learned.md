Lessons Learned

Overview

Building CloudServe provided practical experience in designing, deploying, troubleshooting and validating a highly available three-tier architecture on AWS.

The most valuable part of the project was not simply creating AWS resources, but understanding how the components work together and turning deployment problems into repeatable improvements.

1. Validate Before Updating the Auto Scaling Group

Several launch template versions failed because of application formatting, startup and configuration issues.

A more reliable approach was to launch a fresh canary instance and validate it before using the new version in the Auto Scaling Group.

The validation included:

Python syntax and application import.

Gunicorn and systemd service status.

Binding to 0.0.0.0:8000.

The /health endpoint.

The /db-health endpoint.

Database connectivity.

Lesson: A launch template version should not be trusted until a completely new instance passes the full validation process.

2. A Running Application Is Not Always Reachable

At one stage, the Flask process was running, but the target group still reported unhealthy instances.

The application was not listening on the interface and port expected by the load balancer. Configuring Gunicorn to bind to:

0.0.0.0:8000

resolved the issue.

Lesson: Application validation must cover the complete path:

Process → Socket → Private IP → Target Group → Load Balancer

3. Health Checks Should Have Clear Responsibilities

CloudServe uses two separate health endpoints:

/health
/db-health

The target group uses /health to confirm that the web application can respond successfully.

The /db-health endpoint separately verifies Secrets Manager access, private database connectivity and PostgreSQL authentication.

Lesson: Load balancer health checks should test whether the application can serve traffic, while deeper dependency checks should be monitored separately.

4. User Data Should Be Treated as Deployment Code

User Data errors involving indentation, package installation and service startup caused several deployment failures.

The final bootstrap process was improved by adding:

Strict shell error handling.

Python syntax validation.

Application import validation.

Explicit service checks.

Health checks before considering the deployment successful.

Clear logging for troubleshooting.

Lesson: User Data should be written and tested like deployment code, not treated as a collection of setup commands.

5. Secrets and Configuration Should Be Separated

Database credentials were stored in AWS Secrets Manager rather than being included directly in the application code.

Database host, port and database name were provided as runtime configuration, while IAM permissions allowed the EC2 role to retrieve only the required secret.

Lesson: Application code, environment configuration and sensitive credentials should be managed separately using least-privilege access.

6. Troubleshoot One Layer at a Time

When the application was not reachable through the load balancer, the most effective method was to follow the request path in order:

ALB Listener
→ Target Group
→ Security Group
→ Private IP
→ Application Port
→ Running Process

AWS Reachability Analyzer helped verify the network path, while Systems Manager Session Manager provided secure access to private EC2 instances without enabling SSH.

Lesson: Layer-by-layer troubleshooting is more reliable than changing several resources at the same time.

7. High Availability Must Be Tested

High availability was not assumed simply because an Auto Scaling Group and RDS Multi-AZ were configured.

The application tier was tested by terminating an EC2 instance and confirming that the Auto Scaling Group launched a healthy replacement.

The database tier was tested through a planned RDS Multi-AZ failover.

Lesson: High availability should be demonstrated through controlled failure and recovery tests.

8. DNS and Certificates Require Careful Validation

The Route 53 and AWS Certificate Manager configuration involved DNS propagation delays.

Making repeated changes before propagation completed created additional uncertainty.

Lesson: DNS records should be verified carefully and given sufficient propagation time before they are recreated or modified.

9. Monitoring Is Part of the Architecture

CloudWatch metrics, alarms, an operations dashboard and SNS email notifications were added to improve visibility into the environment.

AWS Budgets was also configured to provide cost alerts.

Lesson: A cloud deployment is not operationally complete until health, failure and cost conditions can be detected.

10. Document Completed and Planned Work Honestly

The AWS Console implementation was completed and validated.

Terraform, CI/CD, centralised application logging, AWS WAF, load testing and multi-region disaster recovery remain future improvements.

Lesson: Portfolio documentation should clearly distinguish between implemented, validated and planned capabilities.

Final Reflection

CloudServe demonstrated that building a reliable AWS solution requires more than creating individual services.

The project involved architecture design, network isolation, security configuration, automated deployment, troubleshooting, failure testing, monitoring and documentation.

The main outcome was a repeatable process for moving from an initial architecture design to a validated and explainable cloud environment.
