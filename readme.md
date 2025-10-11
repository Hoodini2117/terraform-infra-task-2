# AZURE PIPELINE
Modules

This project adopts a modular Terraform architecture, where each major infrastructure component is defined as an independent, reusable module. The goal of this design is to ensure scalability, consistency, and maintainability across multiple environments such as development and production.
Each module contains its own configuration logic, input variables, and outputs, allowing components to be easily managed, reused, and integrated with one another.

1. Compute Module

The Compute module is responsible for provisioning and managing virtual machine instances. It defines the essential compute resources — such as EC2 instances or virtual machines — along with their configurations, including instance types, image IDs, and subnet associations.
It outputs key details like instance IDs and public IPs, which are often referenced by other modules. Typical resources in this module include instance creation, security group association, and user data scripts for initialization and configuration.

2. Load Balancer Module

The Load Balancer module handles the provisioning and configuration of load balancers that distribute incoming traffic across multiple compute instances. It defines the load balancer, listeners, and target groups to achieve efficient traffic management and high availability.
This module may also manage related security groups and routing rules. Common outputs include the load balancer’s DNS name and ARN, which can be utilized by networking or application modules.

3. Networking Module

The Networking module provides the foundational infrastructure layer for the entire environment. It defines and manages core network components such as VPCs (or virtual networks), subnets, route tables, and internet gateways.
This module establishes the networking framework that other modules — such as compute and load balancer — depend upon. Its outputs typically include VPC IDs, subnet IDs, and route table references, which are shared with other modules to ensure seamless connectivity within the infrastructure.

4. Nginx Module

The Nginx module automates the deployment and configuration of Nginx on provisioned compute instances. It includes startup scripts for installation, configuration, and serving of static content or application endpoints.
This module can also be extended to manage load balancing, caching, and SSL termination for web-based workloads. It is often used alongside the compute and load balancer modules to deliver a complete and functional web-serving layer.

Environment Structure

Each environment, such as dev and prod, consumes these modules to build infrastructure specific to that stage.
Every environment directory contains its own main.tf, variables.tf, and outputs.tf files that define how modules are instantiated and which variable values are used. This separation ensures that environments are managed independently, preventing unwanted interference between them.

For example, the development environment might use smaller compute instances and limited subnets for cost efficiency, whereas the production environment can leverage larger instances, multiple load balancers, and high-availability configurations.
Each environment maintains its own Terraform state file, ensuring clear isolation and simplifying state management.

Purpose of Modular Design

The modular approach significantly enhances the flexibility and maintainability of the Terraform codebase. It allows teams to:

Reuse the same set of modules across multiple environments.

Simplify updates by maintaining shared logic in a single place.

Enforce consistent infrastructure standards and naming conventions.

Manage environment-specific configurations cleanly through variable inputs.

This structure not only keeps the Terraform configuration organized and scalable but also supports seamless CI/CD integration, enabling automated deployment pipelines to plan and apply changes independently for each environment.
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

