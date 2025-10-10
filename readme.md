# AZURE PIPELINE
Compute Module

The compute module manages the compute layer of the infrastructure.
It provisions virtual machines or instances, attaches security groups, and manages instance profiles or IAM roles when required.
This module typically depends on the networking module to receive subnet and VPC details.

Includes:
Instance definitions and configuration
Security group associations
Optional startup or provisioning scripts
Load Balancer Module
The loadbalancer module is responsible for setting up and configuring the application or network load balancer.
It creates target groups, listener rules, and health checks to distribute traffic across compute instances.

Includes:
Load balancer and listener configuration
Target group registration
Health checks and routing rules
Networking Module
The networking module defines the foundational networking layer used by all other components.
It sets up the VPC (or Virtual Network), subnets, routing tables, and related networking infrastructure.
This is generally the first module to be applied since other modules depend on its outputs.

Includes:
VPC or Virtual Network creation
Public and private subnets
Internet Gateway and route tables
Network security groups
Nginx Module
The nginx module handles the configuration and deployment of an Nginx web or reverse proxy server.
It may use the compute and load balancer outputs to configure routing or serve content.

Includes:
Nginx installation and configuration
Reverse proxy or static site setup

Optional user_data.sh provisioning script
<img width="284" height="856" alt="Screenshot_20251011_000754" src="https://github.com/user-attachments/assets/552517db-1e6f-4918-8e54-cdbb609a9136" />
<img width="1251" height="871" alt="Screenshot_20251010_230132" src="https://github.com/user-attachments/assets/46d375dd-3e76-4bce-9ea4-2e120b0816e1" />
<img width="1419" height="646" alt="Screenshot_20251010_230218" src="https://github.com/user-attachments/assets/e7816837-ff4e-4089-9e12-58874f7d5eae" />

## azure pipeline to implement the terraform infrastructure

This repository features a comprehensive CI/CD pipeline built with Azure Pipelines to automate the deployment of Azure infrastructure using Terraform. It is designed to provide a safe, predictable, and repeatable process for managing Infrastructure as Code (IaC).
The pipeline workflow is triggered automatically upon any commit to the main branch. It begins with a Plan stage where a self-hosted agent initializes Terraform, validates the code's syntax, and runs terraform plan. The resulting execution plan is then saved as a secure artifact, capturing the exact set of proposed changes.
Following a successful plan, the pipeline enters a critical Manual Approval stage. It pauses and requires an authorized team member to review the generated plan. This human-in-the-loop gate prevents accidental or unapproved changes from being deployed.
Once approved, the final Apply stage begins. A new agent job downloads the exact plan artifact created in the first stage and executes terraform apply. This guarantees that the infrastructure changes being made are precisely the ones that were reviewed and approved, ensuring a reliable and secure deployment process.
To function, the pipeline requires some initial setup in Azure DevOps, including a dedicated self-hosted agent pool, a secure Azure service connection, and an "Environment" configured with the necessary approvers.

<img width="1224" height="865" alt="Screenshot_20251011_003756" src="https://github.com/user-attachments/assets/b6289407-c1b8-4e10-9659-d2aa2ad6bd40" />
<img width="393" height="698" alt="image" src="https://github.com/user-attachments/assets/a1d861b7-ebd9-48a7-9ee9-b6979eeec4ed" />

## jenkins pipeline to implement this infrastructure
This Jenkinsfile defines a declarative pipeline that automates the process of deploying infrastructure to Microsoft Azure using Terraform. The pipeline is designed to run on any available Jenkins agent and begins by securely loading an Azure Service Principal and a Terraform admin password from the Jenkins credential store. These secrets are loaded into the build's environment, ensuring they are never exposed in the logs or the code itself.
The workflow executes in sequential stages. The first stage, 'Azure Login', uses the loaded credentials to authenticate with the Azure CLI, establishing a secure session for the subsequent steps. Once authenticated, the pipeline proceeds to the 'Terraform Init and Plan' stage. It navigates into the environments/dev directory, runs terraform init to prepare the workspace, and then executes terraform plan. This generates a preview of the proposed infrastructure changes and saves them to a file named tfplan, ensuring a predictable outcome.
For safety, the final 'Terraform Apply' stage is conditional and does not run by default. It is controlled by a build parameter named APPLY_CHANGES. A user must manually trigger the pipeline and check this boolean parameter to true, which acts as a manual approval gate. If approved, this stage runs terraform apply on the saved tfplan file, executing the exact changes that were previously planned. Finally, a post action ensures the workspace is cleaned after every run, maintaining a tidy environment for the next execution.

<img width="1794" height="936" alt="image" src="https://github.com/user-attachments/assets/b7ec3351-09c1-436e-bdfa-99c525ab98c9" />

<img width="1794" height="936" alt="image" src="https://github.com/user-attachments/assets/70c25512-96b6-4438-955b-8d98df57fed6" />

