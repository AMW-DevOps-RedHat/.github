# RHCSA/RHCE DevOps Projects Portfolio

A comprehensive collection of three interconnected DevOps projects that progressively build skills from basic Linux administration to advanced high availability architectures. Each project focuses on different aspects of system administration, infrastructure as code, and automation.

---

## 📋 Projects Overview

| # | Project | Status | Key Technologies |
|---|---------|--------|------------------|
| 1 | [Enterprise Linux Basics](#project-1-enterprise-linux-basics) | ✅ Complete | KVM/QEMU, Rocky Linux, Prometheus, Grafana, Bash |
| 2 | [Linux Automation Infrastructure](#project-2-linux-automation-infrastructure) | ✅ Complete | Ansible, Docker, PostgreSQL, NGINX, GitHub Actions |
| 3 | [Linux High Availability](#project-3-linux-high-availability) | 🚧 In Progress | Terraform, Ansible, HAProxy, Keepalived, ELK Stack |

---

## Project 1: Enterprise Linux Basics

**Repository:** [`enterprise-linux-basics-Prjct_01`](./enterprise-linux-basics-Prjct_01/)

### Overview
A comprehensive learning environment simulating a small-scale enterprise server environment with three hosts (web, app, db) configured manually and securely. Designed to practice essential Linux system administration skills aligned with RHCSA objectives.

### Architecture
```
┌─────────────────────────────────────────────────────┐
│                    Host Machine                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  │
│  │  ServOneVM  │  │ ServTwoVM   │  │ ServThreeVM │  │
│  │   (Web)     │  │   (App)     │  │    (DB)     │  │
│  │  Rocky 9.5  │  │  Rocky 9.5  │  │  Rocky 9.5  │  │
│  │  3GB RAM    │  │  3GB RAM    │  │  3GB RAM    │  │
│  │  1 vCPU     │  │  1 vCPU     │  │  1 vCPU     │  │
│  │  30GB Disk  │  │  30GB Disk  │  │  30GB Disk  │  │
│  └─────────────┘  └─────────────┘  └─────────────┘  │
│         KVM/QEMU Virtualization                     │
└─────────────────────────────────────────────────────┘
```

### Completed Milestones

#### ✅ Milestone A - VM Setup and User Management
- **VMs_installation_Script.sh**: Automates creation of 3 Rocky Linux VMs using KVM/QEMU and virt-install
- **Users_Creatiations_Configurations.sh**: Interactive user creation with shell selection, sudo privileges, and SSH key distribution

#### ✅ Milestone C - Backup and Monitoring
- **BackUpScript.sh**: Automated backup solution for `/etc` with compression, logging, and 60-day retention
- **Monitoring Stack**:
  - Prometheus for metrics collection
  - Node Exporter for system metrics
  - Grafana for visualization dashboards
  - Pre-configured systemd services and prometheus.yml

### Technologies
- **OS**: Rocky Linux 9.5 (RHEL derivative)
- **Virtualization**: KVM/QEMU, virt-install
- **Monitoring**: Prometheus, Node Exporter, Grafana
- **Scripting**: Bash automation
- **Backup**: tar, cron

### Quick Start
```bash
# Create VMs
cd "Milestone A"
sudo ./VMs_installation_Script.sh

# Create users
sudo ./Users_Creatiations_Configurations.sh

# Setup monitoring (on each VM)
cd "Milestone C"
# Install and configure Prometheus, Node Exporter, Grafana
```

---

## Project 2: Linux Automation Infrastructure

**Repository:** [`linux-automation-infrastructure-Prjct_02`](./linux-automation-infrastructure-Prjct_02/)

### Overview
Automates multi-tier application deployment using Ansible with Infrastructure as Code principles. Provisions and configures Linux VMs for complete application infrastructure with CI/CD integration through GitHub Actions.

### Architecture
```
┌─────────────────────────────────────────────────────────────┐
│                    Ansible Control Node                     │
│                    (AnsibleOneVM)                           │
│                           │                                 │
│         ┌─────────────────┼─────────────────┐               │
│         │                 │                 │               │
│    ┌────▼────┐      ┌─────▼─────┐      ┌──────▼──────┐      │
│    │  Web    │      │    App    │      │     DB      │      │
│    │ WebServ │      │  AppServ  │      │  DataServ   │      │
│    │  NGINX  │      │   Docker  │      │  PostgreSQL │      │
│    │ Reverse │      │    App    │      │  Hardened   │      │
│    │  Proxy  │      │ Container │      │             │      │
│    └─────────┘      └───────────┘      └─────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

### Completed Components

#### ✅ Infrastructure
- VM provisioning using KVM/libvirt with cloud-init
- Ansible inventory with host groups (`[web]`, `[app]`, `[db]`, `[infra]`)
- Ansible Vault for secrets management

#### ✅ Ansible Playbooks
| Playbook | Purpose |
|----------|---------|
| `site.yml` | Main orchestration playbook |
| `create-vms.yml` | VM creation with virt-install |
| `db_hardening.yml` | PostgreSQL security hardening |
| `docker_setup.yml` | Docker installation & app deployment |

#### ✅ Ansible Roles
- `db_hardening`: PostgreSQL installation and security configuration
- `docker_setup`: Docker installation and containerized application deployment
- `reverse_proxy`: NGINX reverse proxy configuration

#### ✅ CI/CD (GitHub Actions)
- `lint.yml`: Ansible linting on code changes
- `idempotency.yml`: Playbook idempotency testing
- `docker_deploy.yml`: Automated Docker deployment

### Technologies
- **Configuration Management**: Ansible
- **Containerization**: Docker
- **Database**: PostgreSQL (hardened)
- **Web Server**: NGINX
- **CI/CD**: GitHub Actions
- **Secrets**: Ansible Vault

### Quick Start
```bash
# Install required collections
ansible-galaxy collection install community.postgresql community.general

# Run main playbook
ansible-playbook -i inventories/hosts.ini playbooks/site.yml \
  --vault-password-file .vault_pass.txt
```

---

## Project 3: Linux High Availability

**Repository:** [`linux-high-availability-Prjct_03`](./linux-high-availability-Prjct_03/)

### Overview
Hands-on DevOps project focused on building a **High Availability (HA) architecture on cloud platforms**. Uses Terraform for infrastructure provisioning, Ansible for configuration management, HAProxy + Keepalived for redundancy, and ELK Stack for observability.

### Architecture
```
┌──────────────────────────────────────────────────────┐
│                         Cloud VPC/VNet               │
│  ┌────────────────────────────────────────────────┐  │
│  │                    Public Subnet               │  │
│  │  ┌─────────────┐           ┌─────────────┐     │  │
│  │  │   lb1       │ ◄───────► │    lb2      │     │  │
│  │  │  HAProxy    │   VRRP    │   HAProxy   │     │  │
│  │  │ Keepalived  │   VIP     │  Keepalived │     │  │
│  │  └──────┬──────┘           └──────┬──────┘     │  │
│  └─────────│─────────────────────────│────────────┘  │
│            │                         │               │
│  ┌─────────│─────────────────────────│────────────┐  │
│  │                    Private Subnet              │  │
│  │  ┌──────▼──────┐           ┌──────▼──────┐     │  │
│  │  │    web1     │           │    web2     │     │  │
│  │  │    Nginx    │ ◄───────► │    Nginx    │     │  │
│  │  │             │   rsync   │             │     │  │
│  │  └─────────────┘           └─────────────┘     │  │
│  └────────────────────────────────────────────────┘  │
│                                                      │
│  ┌────────────────────────────────────────────────┐  │
│  │                    ELK Stack (Docker)          │  │
│  │  Elasticsearch │ Logstash │ Kibana │ Filebeat  │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

### Project Roadmap

#### ✅ Milestone 0: Foundations (In Progress)
- [x] Configure Terraform Provider & Variables
- [x] Create VPC/VNet, Subnets, Route Table, Security Groups
- [x] Provision 4 VM Instances with Static Private IPs
- [ ] Configure Ansible Inventory and Test Connectivity

#### 🚧 Milestone 1: Core HA Architecture (Pending)
- [ ] Deploy HAProxy on Load Balancers
- [ ] Configure HAProxy Frontend and Backend Pools
- [ ] Enable HAProxy Stats Page for Monitoring
- [ ] Install and Configure Keepalived for VIP
- [ ] Test VIP Failover Between Load Balancers

#### ⏳ Milestone 2: Backend & Data Sync (Pending)
- [ ] Deploy Nginx on Web Servers
- [ ] Configure HAProxy to Balance Web Servers
- [ ] Implement File Synchronization (rsync/DRBD)
- [ ] Test Data Consistency and Failover

#### ⏳ Milestone 3: Observability & Logging (Pending)
- [ ] Deploy ELK Stack with Docker
- [ ] Configure Log Forwarding with Filebeat
- [ ] Validate Logs in Kibana and Build Dashboard

#### ⏳ Milestone 4: Container Orchestration (Optional)
- [ ] Initialize Docker Swarm Cluster
- [ ] Deploy Sample Web App as Docker Service
- [ ] Configure Health Checks and Service Recovery

#### ⏳ Milestone 5: Disaster Recovery & Cleanup (Pending)
- [ ] Simulate Load Balancer Failure
- [ ] Test Web Server Data Recovery
- [ ] Implement Terraform Destroy and Cleanup
- [ ] Write Final Documentation

### Technologies
- **IaC**: Terraform (AWS provider ~> 5.0, region: eu-west-1)
- **Configuration**: Ansible
- **Load Balancing**: HAProxy
- **High Availability**: Keepalived (VRRP)
- **Web Server**: Nginx
- **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana, Filebeat)
- **Orchestration**: Docker Swarm (optional)
- **Data Sync**: rsync

### Quick Start
```bash
# Clone and navigate
git clone https://github.com/AhmadMWaddah/linux-high-availability-Prjct_03.git
cd linux-high-availability-Prjct_03/

# Initialize and apply Terraform
cd lha-terraform
terraform init
terraform apply

# Configure and run Ansible
cd ../lha-ansible
ansible all -m ping -i inventories/hosts.ini
```

---

## 🎯 Learning Path Summary

This project collection follows a progressive learning path:

```
Linux Basics → Automation → High Availability
     │              │              │
     ▼              ▼              ▼
  RHCSA Skills   RHCE Skills   DevOps/SRE Skills
     │              │              │
     ├─ VM Mgmt     ├─ Ansible     ├─ Terraform
     ├─ Users       ├─ Playbooks   ├─ HA Architecture
     ├─ Bash        ├─ Roles       ├─ Load Balancing
     ├─ Monitoring  ├─ Docker      ├─ Failover
     └─ Backups     └─ CI/CD       └─ ELK Stack
```

---

## 📊 Skills Demonstrated

| Category | Skills |
|----------|--------|
| **Linux Administration** | User management, systemd services, file permissions, SSH, cron |
| **Virtualization** | KVM/QEMU, virt-install, cloud-init |
| **Infrastructure as Code** | Terraform (AWS), Ansible playbooks and roles |
| **Configuration Management** | Ansible, Ansible Vault, inventory management |
| **Containerization** | Docker, Docker Swarm, containerized applications |
| **Web Servers** | NGINX configuration, reverse proxy |
| **Databases** | PostgreSQL installation, hardening, security |
| **High Availability** | HAProxy, Keepalived, VRRP, VIP failover |
| **Monitoring & Logging** | Prometheus, Grafana, ELK Stack, Filebeat |
| **Scripting** | Bash automation, validation, error handling |
| **CI/CD** | GitHub Actions, linting, idempotency testing |
| **Backup & Recovery** | Automated backups, retention policies, disaster recovery |

---

## 📁 Repository Structure

```
RHCSA_RHCE_GitHub/
├── README.md                              # This file
├── RHCSA RHCE Project.md                  # Detailed project documentation
├── enterprise-linux-basics-Prjct_01/      # Project 1
│   ├── Milestone A/                       # VM setup & user management
│   └── Milestone C/                       # Backup & monitoring
├── linux-automation-infrastructure-Prjct_02/  # Project 2
│   ├── playbooks/                         # Ansible playbooks
│   ├── roles/                             # Ansible roles
│   ├── inventories/                       # Inventory files
│   └── .github/                           # GitHub Actions workflows
└── linux-high-availability-Prjct_03/      # Project 3
    ├── lha-terraform/                     # Terraform configurations
    ├── lha-ansible/                       # Ansible configurations
    └── Media/                             # Documentation images
```

---

## 🚀 Getting Started

### Prerequisites
- Linux host with KVM/libvirt support (Projects 1 & 2)
- AWS account with appropriate permissions (Project 3)
- Required tools:
  ```bash
  # Core tools
  ansible
  terraform
  python3-pip
  
  # Virtualization (Projects 1 & 2)
  qemu-kvm
  libvirt
  virt-manager
  
  # Ansible collections
  ansible-galaxy collection install community.general community.postgresql
  ```

### Recommended Order
1. Start with **Project 1** for Linux fundamentals
2. Move to **Project 2** for automation with Ansible
3. Complete with **Project 3** for high availability architecture

---

## 📜 License

All projects are licensed under the MIT License – free to use and modify for learning purposes.

---

## 🤝 Contributing

These projects are primarily practice labs for learning purposes. Feel free to:
- Fork and experiment
- Suggest improvements via issues
- Extend milestones with additional features

---
