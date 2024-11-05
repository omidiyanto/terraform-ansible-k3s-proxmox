# Terraform + Ansible to Provision K3S Cluster on Proxmox Automatically
<div align="center">
    <!-- Your badges here -->
    <img src="https://img.shields.io/badge/kubernetes-blue?style=for-the-badge&logo=kubernetes&logoColor=white">
    <img src="https://img.shields.io/badge/terraform-%238511FA.svg?style=for-the-badge&logo=terraform&logoColor=white">
    <img src="https://img.shields.io/badge/ansible-%23000.svg?style=for-the-badge&logo=ansible&logoColor=white">
    <img src="https://img.shields.io/badge/proxmox-%23FF6F00.svg?style=for-the-badge&logo=proxmox&logoColor=white">
    <img src="https://img.shields.io/badge/ubuntu-%23D00000.svg?style=for-the-badge&logo=ubuntu&logoColor=white">
</div>
<br>
Welcome to the <b>K3S Lightweight Kubernetes Cluster Automation on Proxmox VE with Terraform and Ansible</b> project! This repository is designed to help you effortlessly set up a robust Kubernetes (K3S) cluster using Terraform and Ansible. If you're looking to streamline your K3S deployment process on Proxmox Virtual Environment, you’re in the right place!

# 📖 Project Overview
<img src=https://github.com/user-attachments/assets/cbe22844-e705-4e43-ad32-2540c02dcbd7>
In this project, you will find a comprehensive solution for automating the creation of a Kubernetes (K3S) cluster that consists of one master node and two worker nodes. By leveraging Infrastructure as Code (IaC) and configuration management tools, you can set up a scalable environment for deploying your containerized applications with minimal effort.

### 🚀 Key Features
- **Terraform**: Utilize Terraform for provisioning and managing the Proxmox virtual machines, enabling consistent and repeatable deployments.
- **Ansible**: Use Ansible playbooks to automate the installation and configuration of Kubernetes components, ensuring a smooth and efficient setup process.
- **K3S**: Deploy a Lightweight kubernetes cluster ready for your containerized applications.

### Prerequisites
Before you begin, ensure you have the following set up:

- A running **Proxmox VE** environment.
- **Terraform** and **Ansible** installed on proxmox machine.
- Pre-configured VM Template with cloud-init.
- Proxmox API token ID and secret

### Steps
0. Clone this repository
```bash
git clone https://github.com/omidiyanto/terraform-ansible-k3s-proxmox.git
cd terraform-ansible-k3s-proxmox
```

1. Fill required variables on terraform.tfvars
```bash
mv example.terraform.tfvars terraform.tfvars
vim terraform.tfvars
```

2. Initialize Terraform Providers
```bash
terraform init
```

3. Start provision the infrastructe with terraform, and wait until finished
```bash
terraform apply 
```

4. The k3s Cluster should be provisioned already, SSH into the master node and validate the k3s cluster
```bash
ssh YOUR_CLOUDINIT_USER@IP_ADDR_MASTER_NODE
kubectl get nodes
```

# Install MetalLB as the Load Balancer and HAproxy as Ingress Controller
1. Clone this repo inside the master node
```bash
ssh YOUR_CLOUDINIT_USER@IP_ADDR_MASTER_NODE
git clone https://github.com/omidiyanto/terraform-ansible-k3s-proxmox.git
cd terraform-ansible-k3s-proxmox
```
2. Install MetalLB by manifest
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.8/config/manifests/metallb-native.yaml
```
3. Change IP Address Pool range
```bash
vim ipaddresspool.yml
```
4. Apply IP Address Pool and L2advertisement
```bash
kubectl apply -f ipaddresspool.yml
kubectl apply -f L2advertisement.yml
```
5. Install HAproxy as Ingress Controller by manifest
```bash
kubectl apply -f haproxy-ingress-controller.yml
```
# DONE