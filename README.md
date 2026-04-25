# AWS Observability & Governance (Terraform)

This repository demonstrates a production-grade monitoring and compliance architecture provisioned using **Terraform**. It follows the **Observability & Governance Pattern**, a foundational requirement for the AWS Certified Solutions Architect - Associate (SAA-C03) exam.

## 🏗️ Architecture Overview

The infrastructure is designed to provide complete visibility, auditing, and automated compliance across an AWS environment. The architecture consists of:

1.  **Auditing Layer:** **AWS CloudTrail** captures every API call made in the account, providing an immutable record for security and forensic analysis.
2.  **Monitoring Layer:** **Amazon CloudWatch** provides centralized logging and real-time metric alarms (e.g., CPU Utilization) to enable proactive infrastructure management.
3.  **Compliance Layer:** **AWS Config** continuously monitors and records resource configurations, using automated rules to detect non-compliant resources (e.g., unencrypted S3 buckets).
4.  **Storage Layer:** A centralized, policy-secured **Amazon S3** bucket acts as the durable vault for all audit trails and configuration history.

## 🛠️ SAA-C03 Design Patterns Covered

- **Design Secure Architectures (Domain 2):** 
    - **Governance and Compliance:** Implements AWS Config to ensure all resources adhere to organizational security standards.
    - **Forensic Auditing:** Uses CloudTrail to ensure every action is tracked and attributed.
- **Design Resilient Architectures (Domain 1):** 
    - **Proactive Monitoring:** Uses CloudWatch Alarms to detect performance degradation before it leads to system failure.
    - **Automated Remediation:** Sets the foundation for EventBridge-driven automated responses to configuration changes.

## 🚀 Technical Components

- **Audit:** CloudTrail Trail with Multi-Region logging and S3 integration.
- **Monitoring:** CloudWatch Log Groups and Metric Alarms with threshold triggers.
- **Governance:** AWS Config Recorder, Delivery Channels, and Managed Compliance Rules.
- **Identity:** Specialized IAM Service Roles for automated governance tasks.
- **Infrastructure as Code:** 100% automated via Terraform with a state-managed lifecycle.

## 💻 Local Development

This project is optimized for testing using **LocalStack**.

### Prerequisites
- [Terraform](https://www.terraform.io/downloads)
- [Docker](https://www.docker.com/products/docker-desktop)
- [LocalStack](https://localstack.cloud/)

### Deployment
1. Start LocalStack: `docker compose up -d`
2. Initialize Terraform: `terraform init`
3. Deploy Infrastructure: `terraform apply -auto-approve`

---

💡 **Pro Tip: Using `aws` instead of `awslocal`**

If you prefer using the standard `aws` CLI without the `awslocal` wrapper or repeating the `--endpoint-url` flag, you can configure a dedicated profile in your AWS config files.

### 1. Configure your Profile
Add the following to your `~/.aws/config` file:
```ini
[profile localstack]
region = us-east-1
output = json
# This line redirects all commands for this profile to LocalStack
endpoint_url = http://localhost:4566
```

Add matching dummy credentials to your `~/.aws/credentials` file:
```ini
[localstack]
aws_access_key_id = test
aws_secret_access_key = test
```

### 2. Use it in your Terminal
You can now run commands in two ways:

**Option A: Pass the profile flag**
```bash
aws iam create-user --user-name DevUser --profile localstack
```

**Option B: Set an environment variable (Recommended)**
Set your profile once in your session, and all subsequent `aws` commands will automatically target LocalStack:
```bash
export AWS_PROFILE=localstack
aws iam create-user --user-name DevUser
```

### Why this works
- **Precedence**: The AWS CLI (v2) supports a global `endpoint_url` setting within a profile. When this is set, the CLI automatically redirects all API calls for that profile to your local container instead of the real AWS cloud.
- **Convenience**: This allows you to use the standard documentation commands exactly as written, which is helpful if you are copy-pasting examples from AWS labs or tutorials.
