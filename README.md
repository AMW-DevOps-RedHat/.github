# Comprehensive DevOps Projects Overview

This document provides an overview of three interconnected DevOps projects that progressively build skills from basic Linux administration to advanced high availability architectures. Each project focuses on different aspects of system administration and infrastructure as code.

## Project 1: Enterprise Linux Basics (Prjct_01)

### Project Overview

This project is a comprehensive learning environment that simulates a small-scale enterprise server environment with three hosts (web, app, db) configured manually and securely. It's designed to help users practice essential Linux system administration skills needed in most entry-level jobs and RHCSA-aligned roles.

The project is organized into multiple milestones, each focusing on different aspects of Linux system administration:

- **Milestone A**: Virtual Machine setup and user management
- **Milestone B**: (Currently empty)
- **Milestone C**: Backup and monitoring solutions

### Project Structure

```
├── README.md                   # Project overview
├── RedHat Linux Practice.png   # Visual representation of the project
├── Milestone A/
│   ├── Users_Creatiations_Configurations.sh  # User management script
│   ├── VMs_installation_Script.sh           # VM creation automation
│   └── VMs Users/                           # Directory for VM-related files
├── Milestone B/                             # (Empty)
└── Milestone C/
    ├── BackUpScript.sh                      # Automated backup solution
    ├── Grafana Dashboard.png               # Monitoring dashboard screenshot
    ├── Grafana Imported Dashboard.png      # Additional dashboard screenshots
    ├── Grafana Login.png                   # Grafana login screenshot
    ├── Grafana Monitoring.png              # Grafana monitoring screenshot
    ├── grafana.repo                        # Grafana repository configuration
    ├── iftop.png                           # Network monitoring screenshot
    ├── node_exporter.service               # Systemd service for node exporter
    ├── prometheus.service                  # Systemd service for Prometheus
    ├── prometheus.yml                      # Prometheus configuration
    └── sar.png                             # SAR monitoring screenshot
```

### Key Components

#### Milestone A - VM Setup and User Management (FINISHED)

- **VMs_installation_Script.sh**
  - Automates the creation of 3 Rocky Linux minimal server VMs using KVM/QEMU and virt-install
  - Configures VMs with 3GB RAM, 1 vCPU, 30GB disk space
  - Uses Rocky Linux 9.5 ISO for installation
  - Creates VMs named ServOneVM, ServTwoVM, and ServThreeVM

- **Users_Creatiations_Configurations.sh**
  - Interactive script for creating users with shell selection
  - Offers optional sudo privileges during user creation
  - Copies SSH keys from a main user to newly created users
  - Includes validation for usernames and proper error handling
  - Supports both single and multiple user creation modes

#### Milestone B - (NOT FINISHED)
- Currently empty milestone with no implemented components

#### Milestone C - Backup and Monitoring (FINISHED)

- **BackUpScript.sh**
  - Automated backup solution for /etc directory
  - Creates compressed tar.gz backups with date stamps
  - Maintains logs in /var/log/backup.log
  - Cleans up backups older than 60 days

- **Monitoring Stack**
  - **Prometheus**: Metrics collection and storage
  - **Node Exporter**: System metrics exporter for Linux hosts
  - **Grafana**: Visualization dashboard for monitoring metrics
  - Includes pre-configured prometheus.yml targeting multiple hosts
  - Contains systemd service files for both Prometheus and Node Exporter

### Technologies Used
- **Linux Distribution**: Rocky Linux (Red Hat Enterprise Linux derivative)
- **Virtualization**: KVM/QEMU with virt-install
- **Monitoring**: Prometheus, Node Exporter, Grafana
- **Scripting**: Bash automation
- **Backup**: tar, cron for automation

### Usage Instructions
#### Setting up VMs (Milestone A)
1. Ensure KVM, libvirt, and virt-manager are installed on your host system
2. Adjust the ISO path and configuration variables in `VMs_installation_Script.sh`
3. Run the script with sudo privileges to create the VMs
4. Use `Users_Creatiations_Configurations.sh` to manage users on the VMs

#### Setting up Monitoring (Milestone C)
1. Install Prometheus and Node Exporter on target systems
2. Configure the node_exporter.service and prometheus.service as systemd services
3. Use the provided prometheus.yml configuration
4. Set up Grafana for visualization using the included repository configuration

#### Backup Automation
1. Schedule `BackUpScript.sh` with cron for regular automated backups
2. Ensure the backup directory `/backups` exists and has appropriate permissions
3. Monitor `/var/log/backup.log` for backup status

### Educational Goals
This project is designed to provide hands-on experience with:
- Linux system administration tasks
- VM management with KVM/QEMU
- User management and security
- Backup strategies and automation
- System monitoring with Prometheus/Grafana
- Scripting for automation
- Enterprise-level Linux server configurations

---

## Project 2: Linux Automation Infrastructure (Prjct_02)

### Project Overview

This is a Linux automation infrastructure project (Prjct_02) that automates multi-tier application deployment using Ansible. The project provisions and configures Linux VMs for a complete application infrastructure including web, application, and database services, with CI/CD integration through GitHub Actions.

### Key Features:
- VM Provisioning using KVM/libvirt
- Docker setup and containerized application deployment
- NGINX reverse proxy configuration
- PostgreSQL database hardening
- Ansible Vault for secrets management
- GitHub Actions for linting and idempotency testing

### Architecture (FINISHED)
- **Web Server VM** (WebServVM): Hosts reverse proxy and web applications
- **Application Server VM** (AppServVM): Hosts application containers via Docker
- **Database Server VM** (DataBaseServVM): PostgreSQL database with security hardening
- **Ansible Control Node VM** (AnsibleOneVM): Used to run Ansible playbooks

### Directory Structure

```
.
├── .github/                    # GitHub Actions workflows
├── cloud-init/                 # Cloud-init configuration for VM provisioning
│   ├── user-data              # User configuration for VM creation
│   └── meta-data              # Metadata configuration for VM creation
├── config/                     # Additional configuration files
├── images/                     # VM images and seed ISO files
├── inventories/                # Ansible inventory & variables
│   └── hosts.ini              # Host definitions
├── playbooks/                  # Ansible playbooks
│   ├── site.yml               # Main configuration playbook
│   ├── create-vms.yml         # VM creation playbook
│   ├── db_hardening.yml       # Database hardening playbook
│   └── docker_setup.yml       # Docker setup playbook
├── roles/                      # Ansible Galaxy roles
│   ├── db_hardening/          # PostgreSQL hardening
│   ├── docker_setup/          # Docker installation & app deployment
│   └── reverse_proxy/         # NGINX reverse proxy configuration
├── ansible.cfg                 # Ansible configuration file
├── .ansible.cfg               # Additional Ansible configuration
├── .gitignore                 # Git ignore patterns
└── README.md                   # Project documentation
```

### Configuration Files (FINISHED)
- **ansible.cfg**
  - Sets roles path to `./roles`
  - Specifies default inventory as `inventories/hosts.ini`
  - Disables host key checking
  - Sets vault password file to `.vault_pass.txt`
  - Configures SSH connection to use `/home/amw/.ssh/config`

- **hosts.ini (Inventory)**
  - Defines groups of servers:
    - `[web]` for web servers (WebServVM)
    - `[db]` for database servers (DataBaseServVM)
    - `[app]` for application servers (AppServVM)
    - `[infra]` for infrastructure servers (AnsibleOneVM)

### Playbooks (FINISHED)
- `site.yml`: Main orchestration playbook that applies roles to respective VMs
- `create-vms.yml`: Creates VMs using virt-install and cloud-init
- `db_hardening.yml`: PostgreSQL hardening playbook
- `docker_setup.yml`: Docker installation and application setup

#### Execution
Main configuration command:
```bash
ansible-playbook -i inventories/hosts.ini playbooks/site.yml --vault-password-file .vault_pass.txt
```

### Ansible Roles (FINISHED)
- `db_hardening`: Installs and hardens PostgreSQL database
- `docker_setup`: Installs Docker and deploys application containers
- `reverse_proxy`: Configures NGINX reverse proxy

### CI/CD Workflows (FINISHED)
- `lint.yml`: Performs Ansible linting on code changes
- `idempotency.yml`: Tests playbook idempotency
- `docker_deploy.yml`: Automates Docker deployment processes

### Prerequisites (FINISHED)
- **System Requirements**
  - Ansible (latest version)
  - Python 3 with pip
  - libvirt/KVM (for VM provisioning)
  - SSH access to target VMs

- **Ansible Collections**
  - Required collections:
    - community.postgresql
    - community.general
  - Installation command:
    ```bash
    ansible-galaxy collection install community.postgresql community.general
    ```

### Secrets Management (FINISHED)
- Sensitive data is stored encrypted in:
  - `inventories/group_vars/db/secrets.yml` (encrypted with Ansible Vault)

- Manage vault files with:
  - `ansible-vault edit inventories/group_vars/db/secrets.yml`
  - `ansible-vault view inventories/group_vars/db/secrets.yml`

### Cloud-Init Configuration (FINISHED)
- VMs are provisioned using cloud-init with:
  - `user-data`: Defines users, SSH keys, and packages to install
  - `meta-data`: Provides instance metadata

### Development Conventions (FINISHED)
- Use Ansible best practices for playbook and role structure
- Store sensitive data in encrypted vault files
- Test changes with idempotency workflows
- Follow role separation for different infrastructure components
- Use consistent variable naming across playbooks

### Working with This Project (FINISHED)
1. Ensure VMs are created with correct IP addresses
2. Update inventory file with VM IP addresses
3. Configure SSH access in `~/.ssh/config`
4. Provide vault password file if using encrypted variables
5. Run main configuration with `site.yml` playbook

This project is designed for learning and implementing Linux automation with Ansible, focusing on infrastructure as code principles and security best practices.

---

## Project 3: AWS High Availability (Prjct_03)

### Project Overview

This is a hands-on DevOps and Linux Administration practice project focused on building a **High Availability (HA) architecture on AWS**. The project uses **Terraform** for infrastructure provisioning, **Ansible** for configuration management, **HAProxy + Keepalived** for redundancy, and **ELK Stack** for observability.

The project simulates real-world DevOps workflows and practices, structured into 5 milestones that progressively build a complete HA infrastructure with failover, monitoring, and disaster recovery capabilities.

### Architecture Components

- **AWS VPC** with public/private subnets
- **4 EC2 Instances**:
  - `lb1`, `lb2` → HAProxy + Keepalived (VIP failover)
  - `web1`, `web2` → Nginx web servers with data sync
- **Terraform** → Infrastructure as Code (IaC)
- **Ansible** → Automated configuration and deployment
- **ELK Stack** → Centralized logging and observability
- **Optional** → Docker Swarm for container orchestration

### Project Structure

#### Milestone 0: Foundations (FINISHED)
- Configure Terraform Provider & Variables
- Create VPC, Subnets, Route Table, and Security Groups
- Provision 4 EC2 Instances with Static Private IPs
- Configure Ansible Inventory and Test Connectivity

#### Milestone 1: Core HA Architecture (IN PROGRESS)
- Deploy HAProxy on Load Balancers
- Configure HAProxy Frontend and Backend Pools
- Enable HAProxy Stats Page for Monitoring
- Install and Configure Keepalived for VIP
- Test VIP Failover Between Load Balancers

#### Milestone 2: Backend & Data Sync (NOT FINISHED)
- Deploy Nginx on Web Servers
- Configure HAProxy to Balance Web Servers
- Implement File Synchronization Between Web Servers
- Test Data Consistency and Failover

#### Milestone 3: Observability & Logging (NOT FINISHED)
- Deploy ELK Stack with Docker
- Configure Log Forwarding with Filebeat
- Validate Logs in Kibana and Build a Dashboard

#### Milestone 4: Container Orchestration (NOT FINISHED)
- Initialize Docker Swarm Cluster
- Deploy a Sample Web App as Docker Service
- Configure Health Checks and Validate Service Recovery

#### Milestone 5: Disaster Recovery & Cleanup (NOT FINISHED)
- Simulate Load Balancer Failure and Validate VIP Failover
- Test Web Server Data Recovery
- Implement Terraform Destroy and Cleanup
- Write Final Documentation and Lessons Learned

### Building and Running

#### Prerequisites
- AWS account with appropriate permissions
- Terraform installed
- Ansible installed
- SSH key pair for EC2 instances

#### Quick Start Steps
1. Clone the repository
2. Configure AWS credentials
3. Initialize Terraform in the terraform directory
4. Configure Ansible inventory with the provisioned instance IP addresses
5. Run Ansible playbooks to configure the infrastructure

#### Terraform Configuration
The project contains a `providers.tf` file that configures the AWS provider:
- AWS region: eu-west-1 (Ireland)
- AWS provider version: ~> 5.0

### Tools Used (FINISHED)
- **Terraform** – Infrastructure as Code
- **Ansible** – Configuration Management
- **HAProxy + Keepalived** – High Availability
- **Nginx** – Web Servers
- **ELK Stack** – Observability & Logging
- **Docker Swarm (Optional)** – Container Orchestration
- **GitHub Projects & Issues** – Task management

### Project Status (SUMMARY)
- Milestone 0: Foundations – Ready
- Milestone 1: Core HA Architecture – In Progress
- Milestone 2: Backend & Data Sync – Pending
- Milestone 3: Observability & Logging – Pending
- Milestone 4: Container Orchestration – Optional
- Milestone 5: Disaster Recovery & Cleanup – Pending

### Development Conventions (FINISHED)
This project follows DevOps best practices:
- Infrastructure as Code using Terraform
- Configuration management with Ansible
- High availability through load balancing and failover mechanisms
- Centralized logging and monitoring
- Proper security group configuration for network security
- Use of VRRP protocol for VIP failover between load balancers

### File Structure
- `README.md` – Main project documentation
- `RoadMap.md` – Detailed milestone breakdown
- `providers.tf` – Terraform AWS provider configuration
- `.gitignore` – Files and directories to exclude from Git
- `LICENSE` – MIT License terms
- `SSH_Keys/` – Directory for SSH key storage (git-ignored)
- `AWS Credentials/` – Directory for AWS credentials (git-ignored)

### Key Learning Objectives (FINISHED)
- High Availability architecture design and implementation
- Infrastructure as Code with Terraform
- Configuration management with Ansible
- Load balancing with HAProxy
- Failover mechanisms with Keepalived
- Data synchronization between servers
- Centralized logging and observability
- Container orchestration (optional advanced topic)
- Disaster recovery and cleanup procedures

---
