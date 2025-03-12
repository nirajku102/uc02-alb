# uc02-alb

This project demonstrates how to deploy a nginx web server using **AWS (Amazon Web Services)**. The application consists of three components:
1. A **homepage** served at the root path (`/`).
2. An **image gallery** served at the `/images` path.
3. A **registration page** served at the `/register` path.
 
Each component is hosted on a separate **EC2 instance** in different **Availability Zones (AZs)**, and an **Application Load Balancer (ALB)** is used to route traffic to the appropriate instance based on the request path. The entire infrastructure is provisioned using **Terraform**, and the deployment process is automated using **GitHub Actions**.

The goal of this project is to:
1. Create a **custom VPC** with **subnets** in multiple Availability Zones (AZs).
2. Launch **three EC2 instances**, each serving a specific path (`/`, `/images`, `/register`).
3. Set up an **Application Load Balancer (ALB)** to route traffic to the appropriate EC2 instance based on the request path.
4. Verify that the ALB distributes traffic correctly and that the EC2 instances serve the expected content.


(**PREREQUISITES)**
Before starting, ensure you have the following:
1. **AWS Account**: An active AWS account with sufficient permissions to create EC2 instances, VPCs, and ALBs.
2. **Terraform Installed**: Install Terraform from [here](https://www.terraform.io/downloads.html).
3. **AWS CLI Installed**: Install the AWS CLI from [here](https://aws.amazon.com/cli/).
4. **SSH Key Pair**: Create an SSH key pair in AWS or use an existing one.
5. **GitHub Repository**: A GitHub repository to host the project and run the CI/CD pipeline.

**Overview***:

The Application consists of three components (instances)
    1. A homepage served at the root path
    2. A Image gallery served at the /images path
    3. A registration page served at the register path

Each compenent are hosted on a saperate EC2 instance in different AZs.
ALB is used to route traffic to the appropriate instance based on the request path.
The infrastructure is provisioned using TERRAFORM and the deployment process is automated
using GITHUB ACTIONS.

**Requirements** :

## **Terraform Configuration**
 
The Terraform configuration consists of the following files:
1. **`main.tf`**: Defines the AWS resources (VPC, subnets, EC2 instances, ALB, etc.).
2. **`variables.tf`**: Contains input variables for the configuration.
3. **`outputs.tf`**: Outputs the ALB DNS name for testing.
4. **`user_data/`**: Contains scripts to install and configure NGINX on the EC2 instances.
 
### **Key Resources**
- **VPC**: A custom VPC with a CIDR block of `10.0.0.0/16`.
- **Subnets**: Three subnets, each in a different AZ.
- **EC2 Instances**: Three instances, each serving a specific path (`/`, `/images`, `/register`).
- **ALB**: An Application Load Balancer with listener rules to route traffic based on the request path.


## **EC2 Instances Setup**
 
Each EC2 instance is configured using **user data scripts** to install and configure NGINX. The scripts are located in the `user_data/` directory:
 
1. **Instance A (Homepage)**:
   - Serves the root path (`/`).
- Script: `user_data/instance_1.sh`.
   - Content: `Welcome to the Homepage`.
 
2. **Instance B (Images)**:
   - Serves the `/images` path.
- Script: `user_data/instance_2.sh`.
   - Content: `Welcome to the Images Page`.
 
3. **Instance C (Register)**:
   - Serves the `/register` path.
- Script: `user_data/instance_3.sh`.
   - Content: `Welcome to the Register Page`.

## **ALB Setup**
 
The ALB is configured with the following listener rules:
1. **Rule 1**: Path `/` → Forward to `homepage-tg` (Instance A).
2. **Rule 2**: Path `/images` → Forward to `images-tg` (Instance B).
3. **Rule 3**: Path `/register` → Forward to `register-tg` (Instance C).

**(AUTOMATION)**

Have used GITHUB ACTIONS to automate the deployment process
The pipeline runs jobs which includes Terraform init Terraform Plan Terraform Apply

**(TESTING)**

Verify that the ALB correctly routes traffic to appropriate EC2 instance
Ensure that each instance serves the expected content for its respective path
 
1. **Get the ALB DNS Name**:
   - After deploying the infrastructure, retrieve the ALB DNS name:
     ```bash
     terraform output alb_dns_name
     ```
 
2. **Test the ALB**:
   - Use `curl` or a browser to test the ALB:
     - **Homepage**:
       ```bash
       curl http://<ALB-DNS>/

## **Cleaning Up**
 
To avoid unnecessary charges, destroy the infrastructure after testing:
```bash
terraform destroy


















