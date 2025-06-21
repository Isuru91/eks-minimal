# Minimal AWS EKS Cluster with Terraform

This project creates a minimal AWS EKS cluster using Terraform with an S3 backend for state management.

## Prerequisites

- AWS CLI configured with appropriate credentials
- Terraform installed (version >= 1.0.0)
- S3 bucket for Terraform state
- DynamoDB table for state locking

## Infrastructure Components

- VPC with public and private subnets
- NAT Gateway for private subnet internet access
- EKS cluster with managed node groups
- Required IAM roles and security groups

## Setup Instructions

1. Create the S3 bucket and DynamoDB table for the Terraform backend: [terraform Documentation](Documentation)

```bash
# Create S3 bucket
aws s3api create-bucket --bucket terraform-state-eks-minimal --region us-east-1

# Enable versioning on the bucket
aws s3api put-bucket-versioning --bucket terraform-state-eks-minimal --versioning-configuration Status=Enabled

# Create DynamoDB table for state locking (Deprecated)
aws dynamodb create-table \
  --table-name terraform-state-lock \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region us-east-1
```

2. Initialize Terraform:

```bash
terraform init
```

3. Review the plan:

```bash
terraform plan
```

4. Apply the configuration:

```bash
terraform apply
```

5. Configure kubectl to interact with your cluster:

```bash
aws eks update-kubeconfig --region us-east-1 --name minimal-eks-cluster
```

## Cleanup

To destroy all resources created by this project:

```bash
terraform destroy
```

## Customization

Edit the `variables.tf` file to customize:
- AWS region
- Cluster name and version
- VPC CIDR and availability zones
- Node instance types and count
