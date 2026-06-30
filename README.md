# Production-Grade Self-Hosted n8n Stack on Hostinger VPS

This repository contains the official deployment architecture blueprints for migrating high-volume automation pipelines off expensive managed cloud platforms (like Zapier and Make) and onto a private, secure, self-hosted infrastructure layer running on a **Hostinger KVM VPS**.

## 🚀 Why This Architecture?

Running advanced AI agents and high-frequency webhook data loops on linear-billing systems introduces a severe "success tax". By leveraging containerized environments inside an isolated virtual private server, you secure:
* **Infinite Scale:** Zero per-task execution fees—your utility costs remain completely flat.
* **Data Sovereignty:** Enterprise compliance control. Your transaction logs, internal data loops, and API keys remain entirely within your own cloud architecture.
* **High Availability:** Built-in automatic database health checks and state recovery.

## 📦 Tech Stack Components

* **Compute:** Hostinger Ubuntu 24.04 LTS KVM VPS
* **Orchestration:** Docker & Docker Compose
* **Automation Backend:** n8n Community Edition (Latest Stable)
* **Storage Layer:** PostgreSQL 16 (Isolated Alpine Volume)

---

## 🛠️ Step-by-Step Deployment Quickstart

### 1. Host Infrastructure Setup
1. Log into your **Hostinger Dashboard** and spin up a clean **Ubuntu 24.04 LTS** instance.
2. Point your custom automation subdomain (e.g., `automation.yourdomain.com`) to your Hostinger VPS static IP address via an **A Record** in your DNS management panel.

### 2. Connect and Provision the Server
Access your virtual slice via your terminal using SSH:
```bash
ssh root@your_hostinger_vps_ip

Update host packages and verify the Docker container ecosystem is deployed:
sudo apt-get update
sudo apt-get install -y docker-compose

3. Initialize the Repository Blueprint
Clone this deployment configuration framework onto your host filesystem:
git clone https://github.com/Nefro-stack/self-hosted-n8n-hostinger.git
cd self-hosted-n8n-hostinger

Instantiate your private runtime environment configuration block:
cp .env.example .env
nano .env

Modify the default values, insert your unique domain name mapping, and replace the placeholder keys with secure passwords.

4. Ignite the Infrastructure Cluster
Launch the isolated database clusters and background workflow engines in detached state:
docker-compose up -d

Your system is now online! Set up a secure reverse proxy layer (such as Nginx Proxy Manager, Caddy, or Cloudflare Tunnels) on your server to handle automatic SSL certificate routing straight into your exposed container port 5678.

Maintained by Pratik Batha | Founder, ApexQuant. To support our developer community, deploy your cloud compute profiles natively through Hostinger.
