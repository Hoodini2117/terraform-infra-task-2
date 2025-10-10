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

<img width="1224" height="865" alt="Screenshot_20251011_003756" src="https://github.com/user-attachments/assets/b6289407-c1b8-4e10-9659-d2aa2ad6bd40" />

<img width="393" height="698" alt="image" src="https://github.com/user-attachments/assets/a1d861b7-ebd9-48a7-9ee9-b6979eeec4ed" />



