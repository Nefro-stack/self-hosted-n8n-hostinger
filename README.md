Self-Hosted n8n Infrastructure on Hostinger (Production-Ready)
Welcome to the infrastructure repository for the ApexQuant self-hosted n8n node. This repo provides the production-grade docker-compose configuration and environment structure required to run n8n at scale on a Hostinger VPS.

We use this exact setup to process tens of thousands of automated tasks monthly—moving away from the compounding costs of Make.com and Zapier to maintain total control over our data and unit economics.

Join 22,000+ founders getting weekly AI and automation playbooks from my newsletter: Subscribe to ApexQuant.

🏗️ Repository Structure
docker-compose.yml: The orchestration file defining the n8n container, the PostgreSQL database instance, and the Traefik reverse proxy.

.env.example: The template for your environment variables. Copy this to a file named .env to configure your instance.

LICENSE: MIT License.

🚀 Deployment Guide
1. Provision the Server
Spin up a Hostinger VPS (KVM1 is sufficient for starting out; KVM2 is recommended for production).

During the OS selection, choose the Ubuntu 24.04 with n8n template.

Point your domain A-record to the VPS IP address.

2. Configure Environment
Navigate to your installation directory (e.g., /opt/n8n) and set up your environment variables:

Bash
# Copy the example to active .env
cp .env.example .env

# Edit your configuration
nano .env
Ensure your WEBHOOK_URL and database credentials are set correctly in the .env file before launching.

3. Launch the Stack
Initialize your production environment with:

Bash
docker compose up -d
⚙️ Advanced Tuning & Performance
High-volume production environments require fine-tuning to prevent memory bottlenecks.

Optimization in .env
Update your .env settings to manage data lifecycle and memory allocation:

Code snippet
# Database Pruning (Crucial for high-volume execution)
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168 
EXECUTIONS_DATA_PRUNE_MAX_COUNT=10000

# Memory Allocation (Adjust based on your KVM plan)
# For KVM2 (8GB RAM), we recommend setting:
NODE_OPTIONS="--max-old-space-size=6144"
After modifying the .env, apply changes using:

Bash
docker compose down && docker compose up -d
🛠️ Maintenance Commands
Action	Command
Check Logs	docker compose logs -f n8n
Update n8n	docker compose pull && docker compose up -d
Clear Storage	docker system prune -a --volumes
Maintained by Pratik Batha.

Get the blueprints and automations we run on this stack at ApexQuant.
