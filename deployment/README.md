# Deployment Files

This folder contains the deployment files used for the CloudServe application tier.

## Contents

- `cloudserve-user-data.sh` — Bootstraps a new EC2 instance, installs the required packages, configures the Flask application and Gunicorn service, and validates the application health.

## Security

Sensitive values such as the AWS account ID, Secrets Manager ARN, RDS endpoint and credentials are replaced with placeholders before publication.
