# Digital Ocean Droplet Setup Guide

Quick guide to prepare your droplet for automated deployments.

## 1️⃣ Create Droplet

- Choose Ubuntu 22.04 LTS
- Select appropriate size ($6/month works fine)
- Add your SSH key
- Note the IP address

## 2️⃣ SSH into Droplet

```bash
ssh root@YOUR_DROPLET_IP
```

## 3️⃣ Install Docker

```bash
# Update system
apt update && apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Verify installation
docker --version
```

## 4️⃣ Configure Firewall

```bash
# Allow SSH, HTTP, HTTPS
ufw allow OpenSSH
ufw allow 80/tcp
ufw allow 443/tcp
ufw enable
```

## 5️⃣ Login to Docker Hub

```bash
docker login
# Enter your Docker Hub credentials
```

## 6️⃣ Generate SSH Key for GitHub Actions

```bash
# Generate key (if not already added during droplet creation)
cat ~/.ssh/authorized_keys

# Copy the private key from your local machine that corresponds to the public key
# This goes into GitHub Secrets as DROPLET_SSH_KEY
```

## 7️⃣ Test Manual Deployment

```bash
# Pull and run your image
docker pull yourusername/devops-training:latest
docker run -d --name devops-app -p 8080:8000 --restart unless-stopped yourusername/devops-training:latest

# Check if running
docker ps
curl http://localhost
```

## 8️⃣ GitHub Secrets Configuration

Add these to your GitHub repository:

**Settings → Secrets and variables → Actions → New repository secret**

```
DOCKER_USERNAME = your-dockerhub-username
DOCKER_PASSWORD = your-dockerhub-password (or access token)
DROPLET_IP = 123.456.789.012
DROPLET_USER = root
DROPLET_SSH_KEY = -----BEGIN RSA PRIVATE KEY-----
                  (your private key)
                  -----END RSA PRIVATE KEY-----
```

## 9️⃣ Verify Deployment

After pushing to GitHub:

1. Go to **Actions** tab in your repository
2. Watch the workflow run
3. Check your droplet: `curl http://YOUR_DROPLET_IP`

## 🔍 Monitoring

```bash
# View logs
docker logs devops-app

# Follow logs
docker logs -f devops-app

# Check container status
docker ps

# Check resource usage
docker stats
```

## 🛑 Stop Application

```bash
docker stop devops-app
docker rm devops-app
```

## 🔄 Manual Update

```bash
docker pull yourusername/devops-training:latest
docker stop devops-app && docker rm devops-app
docker run -d --name devops-app -p 8080:8000 --restart unless-stopped yourusername/devops-training:latest
```
