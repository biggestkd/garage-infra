# Starter Infrastructure Repository

This repository provides a **starter template** for managing infrastructure as code (IaC) using **AWS CloudFormation**.  
It is designed to be the baseline structure for new infrastructure projects, ensuring consistency, modularity, and ease of deployment across apps and systems.

---

## 📂 Repository Structure

At the top level, each folder represents an **app/system**.  
Inside each app/system folder, there are **projects**, and inside projects, there are **resource definitions** (e.g., S3 buckets, IAM roles, policies) with optional environment specifiers.

```
starter-infra-repo/
├── app1/
│ ├── project-a/
│ │ ├── buckets/
│ │ │ ├── bucket.yml
│ │ │ └── bucket-params.json
│ │ ├── roles/
│ │ │ ├── role.yml
│ │ │ └── role-params.json
│ │ └── policies/
│ └── project-b/
│ └── ...
├── app2/
│ └── project-c/
│ └── ...
└── README.md
```

### Naming Conventions
- **App/System folder**: high-level business unit, service, or product (`billing/`, `auth/`, `payments/`).  
- **Project folder**: a logical group of resources (`api/`, `etl/`, `dashboard/`).  
- **Resource folder**: individual resources or resource types (`buckets/`, `roles/`, `policies/`).  

This hierarchy makes it easy to scale from **small projects** to **enterprise-scale infrastructure**.

---

## 🛠️ Prerequisites

Before using this repo, ensure you have the following installed and configured:

- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) (v2 recommended)  
- [AWS Account + IAM permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) to create and manage resources  
  - `cloudformation:*`
  - `iam:*`
  - `s3:*`
  - plus any service-specific permissions  
- (Optional) [AWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) or named CLI profiles for multi-account deployments  
- [jq](https://stedolan.github.io/jq/) (recommended for parameter file editing/templating)

---

## 🚀 Usage

### 1. Navigate to a Resource
Each resource is defined as a **CloudFormation template (`.yml`)**.  
Example:
```bash
cd app1/project-a/buckets
```

### 2. Validate the Template

Before deploying, validate the CloudFormation template:
Example:
```bash
aws cloudformation validate-template --template-body file://bucket.yml
```

### 3. Deploy the Stack

Create or update the stack:
Example:
```bash
aws cloudformation deploy \
  --stack-name my-bucket-stack \
  --template-file bucket.yml \
  --parameter-overrides file://bucket-params.json \
  --capabilities CAPABILITY_NAMED_IAM


--stack-name: logical name of the stack (unique per AWS account/region).

--parameter-overrides: pass values from the params file.

--capabilities: required if the stack defines IAM resources (roles, policies).
```

### 4. Delete a Stack

When no longer needed:
Example:
```bash
aws cloudformation delete-stack --stack-name my-bucket-stack
```

---

💡 *Pro-tip*: Use the folder structure to maintain modular templates (roles, buckets, Lambdas, etc.) for clarity and reuse.  
When you’re ready to deploy, package those templates together into a single artifact—this keeps your source organized while simplifying deployment to AWS.

Example:
```bash
# Package the template and upload nested/child templates & Lambda code to S3
aws cloudformation package \
  --template-file env-parent.yml \
  --s3-bucket my-artifact-bucket \
  --output-template-file env-parent.packaged.yml

# Deploy the packaged template as a stack
aws cloudformation deploy \
  --template-file env-parent.packaged.yml \
  --stack-name garage-dev \
  --parameter-overrides Env=dev App=garage \
  --capabilities CAPABILITY_NAMED_IAM

