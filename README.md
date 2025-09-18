# AWS ECS Infrastructure with Terraform

A complete AWS infrastructure setup using Terraform that deploys an ECS cluster with Auto Scaling, Application Load Balancer, RDS PostgreSQL database, and VPC networking in the Mumbai (ap-south-1) region.

## Directory Structure

```
terraform-ecs-infrastructure/
├── main.tf                 # Complete infrastructure configuration
├── terraform.tfvars       # Variable values (optional)
├── images/                 # Screenshots and diagrams
│   ├── browser-op.png
│   ├── cmd_current_op_terraform.png
│   └── vpc-resource-map.png
└── README.md              # This file
```

## Architecture Overview

This Terraform configuration creates:

- **VPC** with public/private subnets across 2 AZs
- **ECS Cluster** with EC2 capacity provider and auto scaling
- **Application Load Balancer** for traffic distribution
- **RDS PostgreSQL** database in private subnets
- **Security Groups** with proper ingress/egress rules
- **NAT Gateway** for private subnet internet access
- **CloudWatch Logs** for ECS task monitoring

<img width="1919" height="1072" alt="Screenshot 2025-09-18 201942" src="https://github.com/user-attachments/assets/f59d58a7-3c8f-4f8f-b161-2e257f5f8e99" />

*VPC architecture and resource layout*

## Prerequisites

1. **AWS CLI** configured with appropriate credentials
   ```bash
   aws configure
   ```

2. **Terraform** installed (>= 1.0)
   ```bash
   # macOS
   brew install terraform
   
   # Ubuntu/Debian
   sudo apt-get update && sudo apt-get install terraform
   
   # Windows
   choco install terraform
   ```

3. **AWS IAM Permissions** - Your AWS user/role needs permissions for:
   - EC2, VPC, ECS, RDS, IAM, CloudWatch, Auto Scaling, Load Balancer

## Quick Start

### 1. Clone and Setup

```bash
# Create project directory
mkdir terraform-ecs-infrastructure
cd terraform-ecs-infrastructure

# Copy the main.tf file to this directory
# (Paste the Terraform configuration into main.tf)
```

### 2. Initialize Terraform

```bash
terraform init
```

### 3. Plan and Deploy

```bash
# Review what will be created
terraform plan

# Apply the configuration
terraform apply
```

Type `yes` when prompted to confirm the deployment.

<img width="1280" height="800" alt="VirtualBox_sdubantu_18_09_2025_20_14_55" src="https://github.com/user-attachments/assets/c865498b-eb45-4e63-93ad-5f2044edcc3f" />

*Terraform apply command execution and output*

## Workflow Commands

### Initial Deployment
```bash
# Initialize Terraform
terraform init

# Format code
terraform fmt

# Validate configuration
terraform validate

# Plan deployment
terraform plan

# Apply changes
terraform apply
```

### Managing Infrastructure
```bash
# Show current state
terraform show

# List all resources
terraform state list

# Get outputs
terraform output

# Get specific output
terraform output application_url

# Refresh state
terraform refresh
```

### Updates and Changes
```bash
# Plan changes after modifying main.tf
terraform plan

# Apply changes
terraform apply

# Target specific resource for changes
terraform apply -target=aws_ecs_service.app
```

## Configuration Details

### Default Settings

| Component | Configuration |
|-----------|---------------|
| **Region** | ap-south-1 (Mumbai) |
| **VPC CIDR** | 10.0.0.0/16 |
| **EC2 Instance** | t3.micro |
| **RDS Instance** | db.t3.micro (PostgreSQL 16.4) |
| **ECS Task** | 256 CPU, 512 Memory |
| **Auto Scaling** | Min: 1, Max: 3, Desired: 1 |

## Important Notes

### Database Access
```bash
# Get RDS endpoint (sensitive output)
terraform output -raw rds_endpoint

# Database credentials (change in production)
Username: dbadmin
Password: TempPassword123!
Database: appdb
```

## Accessing Your Application

After successful deployment:

```bash
# Get application URL
terraform output application_url
```

Visit the URL in your browser to see the running Nginx container.

<img width="1280" height="800" alt="VirtualBox_sdubantu_18_09_2025_20_22_17" src="https://github.com/user-attachments/assets/4c3f0335-237f-4817-838e-168deea62157" />


*Application running successfully in browser*

## Clean Up Resources

⚠️ **Important**: This will delete all resources and data

```bash
terraform destroy -auto-approve
```

The cleanup process typically takes 5-10 minutes to complete.
