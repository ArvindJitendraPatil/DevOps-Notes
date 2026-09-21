# 🚀 DevOps Setup Guide

A complete step-by-step guide to set up a **DevOps learning and project environment** on Ubuntu/Linux/WSL.

---

## 📋 Table of Contents

1. [System Update](#1-system-update)
2. [Git Installation](#2-git-installation)
3. [GitHub Configuration](#3-github-configuration)
4. [Docker Installation](#4-docker-installation)
5. [Jenkins Installation](#5-jenkins-installation)
6. [AWS CLI Installation](#6-aws-cli-installation)
7. [Terraform Installation](#7-terraform-installation)
8. [Ansible Installation](#8-ansible-installation)
9. [Python Installation](#9-python-installation)
10. [kubectl Installation](#10-kubectl-installation)
11. [Kubernetes with Kind](#11-kubernetes-with-kind)
12. [Useful Docker Commands](#12-useful-docker-commands)
13. [Useful Git Commands](#13-useful-git-commands)
14. [Useful Linux Commands](#14-useful-linux-commands)
15. [DevOps Project Structure](#15-devops-project-structure)
16. [Verification Checklist](#16-verification-checklist)
17. [Troubleshooting](#17-troubleshooting)

---

# 1. System Update

Update Ubuntu packages before installing DevOps tools.

```bash
sudo apt update
sudo apt upgrade -y
```

Check Ubuntu version:

```bash
lsb_release -a
```

Check kernel:

```bash
uname -a
```

---

# 2. Git Installation

Install Git:

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

Configure Git:

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Check configuration:

```bash
git config --global --list
```

---

# 3. GitHub Configuration

## Generate SSH Key

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Press `Enter` to accept the default location.

Start SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add SSH key:

```bash
ssh-add ~/.ssh/id_ed25519
```

Display public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the output and add it to:

**GitHub → Settings → SSH and GPG keys → New SSH key**

Test connection:

```bash
ssh -T git@github.com
```

---

# 4. Docker Installation

## Remove old Docker packages

```bash
sudo apt remove docker.io docker-doc docker-compose podman-docker containerd runc -y
```

## Install prerequisites

```bash
sudo apt update

sudo apt install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release -y
```

## Add Docker GPG key

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## Add Docker repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Install Docker

```bash
sudo apt update
```

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

## Verify Docker

```bash
docker --version
```

```bash
docker compose version
```

## Test Docker

```bash
sudo docker run hello-world
```

## Run Docker without sudo

Add current user to Docker group:

```bash
sudo usermod -aG docker $USER
```

Apply group changes:

```bash
newgrp docker
```

Test:

```bash
docker run hello-world
```

Check Docker service:

```bash
sudo systemctl status docker
```

---

# 5. Jenkins Installation

## Install Java

Jenkins requires Java.

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
```

Verify:

```bash
java -version
```

## Add Jenkins Repository Key

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

Add repository:

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

## Install Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
```

Start Jenkins:

```bash
sudo systemctl start jenkins
```

Enable Jenkins:

```bash
sudo systemctl enable jenkins
```

Check status:

```bash
sudo systemctl status jenkins
```

## Get Initial Admin Password

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Open Jenkins:

```text
http://localhost:8080
```

Paste the initial password and complete the Jenkins setup.

---

# 6. AWS CLI Installation

Download AWS CLI:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" \
-o "awscliv2.zip"
```

Install unzip:

```bash
sudo apt install unzip -y
```

Extract:

```bash
unzip awscliv2.zip
```

Install:

```bash
sudo ./aws/install
```

Verify:

```bash
aws --version
```

Configure:

```bash
aws configure
```

Enter:

```text
AWS Access Key ID:
AWS Secret Access Key:
Default region name:
Default output format:
```

Test:

```bash
aws sts get-caller-identity
```

> **Security:** Never commit AWS access keys or secret keys to GitHub.

---

# 7. Terraform Installation

Install prerequisites:

```bash
sudo apt update
sudo apt install gnupg software-properties-common curl -y
```

Add HashiCorp GPG key:

```bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

Add repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com \
$(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Install Terraform:

```bash
sudo apt update
sudo apt install terraform -y
```

Verify:

```bash
terraform version
```

---

# 8. Ansible Installation

Install Ansible:

```bash
sudo apt update
sudo apt install ansible -y
```

Verify:

```bash
ansible --version
```

Test:

```bash
ansible localhost -m ping
```

Expected result:

```text
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# 9. Python Installation

Check Python:

```bash
python3 --version
```

Install Python:

```bash
sudo apt install python3 python3-pip python3-venv -y
```

Verify:

```bash
python3 --version
```

```bash
pip3 --version
```

## Create Virtual Environment

```bash
python3 -m venv venv
```

Activate:

```bash
source venv/bin/activate
```

Deactivate:

```bash
deactivate
```

---

# 10. kubectl Installation

Download the latest stable kubectl:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s \
https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

Install:

```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Verify:

```bash
kubectl version --client
```

Check configuration:

```bash
kubectl config view
```

---

# 11. Kubernetes with Kind

## Install Kind

```bash
curl -Lo ./kind \
https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-amd64
```

Make executable:

```bash
chmod +x ./kind
```

Move to PATH:

```bash
sudo mv ./kind /usr/local/bin/kind
```

Verify:

```bash
kind version
```

## Create Kubernetes Cluster

```bash
kind create cluster --name devops-cluster
```

Check cluster:

```bash
kind get clusters
```

Check nodes:

```bash
kubectl get nodes
```

Expected:

```text
NAME                         STATUS   ROLES
devops-cluster-control-plane Ready    control-plane
```

Delete cluster:

```bash
kind delete cluster --name devops-cluster
```

---

# 12. Useful Docker Commands

## Check Docker Version

```bash
docker --version
```

## List Running Containers

```bash
docker ps
```

## List All Containers

```bash
docker ps -a
```

## List Images

```bash
docker images
```

## Pull Image

```bash
docker pull nginx
```

## Run Container

```bash
docker run -d --name nginx-container -p 8080:80 nginx
```

## Stop Container

```bash
docker stop nginx-container
```

## Start Container

```bash
docker start nginx-container
```

## Remove Container

```bash
docker rm nginx-container
```

## Remove Image

```bash
docker rmi nginx
```

## View Logs

```bash
docker logs nginx-container
```

## Execute Command Inside Container

```bash
docker exec -it nginx-container /bin/bash
```

## Build Image

```bash
docker build -t my-app:latest .
```

## Run Image

```bash
docker run -d -p 8080:8080 my-app:latest
```

## Docker Compose

```bash
docker compose up -d
```

```bash
docker compose down
```

```bash
docker compose ps
```

```bash
docker compose logs
```

---

# 13. Useful Git Commands

## Initialize Repository

```bash
git init
```

## Check Status

```bash
git status
```

## Add Files

```bash
git add .
```

## Commit

```bash
git commit -m "Add DevOps setup guide"
```

## View Branch

```bash
git branch
```

## Create Branch

```bash
git checkout -b feature/devops-setup
```

## Switch Branch

```bash
git switch main
```

## View Remote

```bash
git remote -v
```

## Add Remote

```bash
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

## Push

```bash
git push origin main
```

## Pull

```bash
git pull origin main
```

## View Commit History

```bash
git log --oneline
```

---

# 14. Useful Linux Commands

## Current Directory

```bash
pwd
```

## List Files

```bash
ls
```

Detailed list:

```bash
ls -la
```

## Change Directory

```bash
cd /path/to/directory
```

## Create Directory

```bash
mkdir devops-project
```

## Create File

```bash
touch README.md
```

## Copy File

```bash
cp file.txt backup.txt
```

## Move File

```bash
mv file.txt /tmp/
```

## Delete File

```bash
rm file.txt
```

## Delete Directory

```bash
rm -rf directory-name
```

## Find Files

```bash
find /path -type f -name "*.log"
```

## Find Files Older Than 90 Days

```bash
find /path -type f -mtime +90
```

## Delete Files Older Than 90 Days

First verify:

```bash
find /path -type f -mtime +90
```

Then delete:

```bash
find /path -type f -mtime +90 -delete
```

> Always verify the files with the first command before using `-delete`.

## Disk Usage

```bash
df -h
```

## Directory Size

```bash
du -sh *
```

## Memory

```bash
free -h
```

## CPU/Processes

```bash
top
```

or:

```bash
htop
```

Install htop:

```bash
sudo apt install htop -y
```

## Check IP

```bash
ip addr
```

## Check Open Ports

```bash
ss -tulnp
```

---

# 15. DevOps Project Structure

A recommended DevOps project structure:

```text
devops-project/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── templates/
│
├── docker/
│   └── Dockerfile
│
├── kubernetes/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── configmap.yaml
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── terraform.tfvars
│
├── ansible/
│   ├── inventory
│   ├── playbook.yml
│   └── roles/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── Jenkinsfile
├── Dockerfile
├── docker-compose.yml
├── README.md
└── .gitignore
```

---

# 16. Verification Checklist

Run the following commands after installation.

## Git

```bash
git --version
```

## Docker

```bash
docker --version
```

```bash
docker compose version
```

## Jenkins

```bash
java -version
```

```bash
sudo systemctl status jenkins
```

## AWS CLI

```bash
aws --version
```

## Terraform

```bash
terraform version
```

## Ansible

```bash
ansible --version
```

## Python

```bash
python3 --version
```

## kubectl

```bash
kubectl version --client
```

## Kind

```bash
kind version
```

---

# 17. Troubleshooting

## Docker Permission Denied

If you see:

```text
permission denied while trying to connect to the Docker daemon
```

Run:

```bash
sudo usermod -aG docker $USER
```

Then:

```bash
newgrp docker
```

Test:

```bash
docker ps
```

---

## Jenkins Not Starting

Check status:

```bash
sudo systemctl status jenkins
```

Check logs:

```bash
sudo journalctl -u jenkins -n 100 --no-pager
```

Check Java:

```bash
java -version
```

Restart Jenkins:

```bash
sudo systemctl restart jenkins
```

---

## Port 8080 Already in Use

Check:

```bash
sudo ss -tulnp | grep :8080
```

Find the process:

```bash
sudo lsof -i :8080
```

---

## Terraform Command Not Found

Check:

```bash
which terraform
```

```bash
terraform version
```

If installation failed:

```bash
sudo apt update
sudo apt install terraform -y
```

---

## kubectl Command Not Found

Check:

```bash
which kubectl
```

```bash
kubectl version --client
```
--

# 🏆 DevOps Tool Stack

```text
Cloud        → AWS
OS           → Linux
Version Ctrl → Git + GitHub
Container    → Docker
CI/CD        → Jenkins + GitHub Actions
IaC          → Terraform
Config Mgmt  → Ansible
Orchestration→ Kubernetes
Cluster Lab  → Kind
Monitoring   → CloudWatch / Dynatrace
Security     → Trivy
Scripting    → Bash + Python
```

---

# 🔐 Security Best Practices

Never commit:

```text
AWS Access Keys
AWS Secret Keys
Passwords
Private SSH Keys
.env files containing secrets
Terraform state containing sensitive data
Kubernetes Secrets in plain text
```

Use `.gitignore`:

```gitignore
.env
*.pem
*.key
.terraform/
terraform.tfstate
terraform.tfstate.*
*.tfvars
__pycache__/
venv/
```

---

# ✅ Final Verification

Run:

```bash
git --version
docker --version
docker compose version
java -version
aws --version
terraform version
ansible --version
python3 --version
kubectl version --client
kind version
```
