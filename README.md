Markdown# Self-Hosted n8n Infrastructure on Hostinger

This repository contains the production-ready infrastructure to run **n8n Community Edition** on a Hostinger VPS. We use this exact stack at [ApexQuant](https://apexquant.substack.com/) to process thousands of automations without the exorbitant costs of cloud platforms like Make.com or Zapier.

**Join 22,000+ founders:** [**Subscribe to ApexQuant**](https://apexquant.substack.com/) for weekly automation playbooks.

## 🚀 Quick Start

### 1. Provision & Clone
1. Spin up a Hostinger VPS with the **Ubuntu 24.04 with n8n** template.
2. SSH into your server and navigate to your folder:
   ```bash
   cd /opt/n8n
2. Configure EnvironmentCopy the example file to a new .env file:Bashcp .env.example .env
Open the file to add your domain and secure passwords:Bashnano .env
3. LaunchInitialize your production stack:Bashdocker compose up -d
🛠️ MaintenanceCommandPurposedocker compose logs -f n8nView live logs for debuggingdocker compose pull && docker compose up -dUpdate to the latest n8n versiondocker system prune -aClean up unused images/volumesMaintained by Pratik Batha.Get the exact AI automation blueprints we use on this stack at ApexQuant.
