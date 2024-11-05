# Terraform + Ansible to Provision K3S Cluster on Proxmox Automatically
## Steps
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