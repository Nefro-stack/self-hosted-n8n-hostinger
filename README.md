# Self-Hosted n8n Infrastructure on Hostinger (Production-Ready)

Welcome to the official infrastructure repository for running a self-hosted **n8n Community Edition** node. We use this setup at [ApexQuant](https://apexquant.substack.com/) to process tens of thousands of automated tasks monthly while maintaining total control over our data and unit economics.

By migrating away from cloud platforms like Make.com or Zapier, this Hostinger VPS architecture creates massive cost leverage for founders and agency owners scaling their operations.

Join 22,000+ founders getting weekly AI and automation playbooks from my newsletter: **[Subscribe to ApexQuant](https://apexquant.substack.com/)**.

---

## 🏗️ System Architecture

This deployment relies on a containerized microservices architecture to ensure stability, easy backups, and seamless scaling. The Hostinger `Ubuntu 24.04 with n8n` template provisions the following stack out of the box:

*   **n8n Node:** The core application engine (Community Edition).
*   **PostgreSQL:** Dedicated relational database for storing workflow states, credentials, and execution history. Much more stable for high-volume execution than the default SQLite setup.
*   **Traefik:** Edge router and reverse proxy. Handles automatic SSL termination and routes incoming webhook traffic securely.
*   **Docker & Docker Compose:** The orchestration layer keeping the services isolated and automatically restarting them on failure.
*   **Let's Encrypt:** Automated TLS/SSL certificate generation and renewal.

---

## 🚀 Deployment Guide

### 1. Provision the Server
1. Spin up a **Hostinger VPS** (KVM1 is sufficient for starting out; KVM2 is recommended for production).
2. During the OS selection phase, choose the **Ubuntu 24.04 with n8n** template.
3. Assign your domain/subdomain to the VPS IP address in your DNS settings (e.g., `n8n.yourdomain.com`).

### 2. Initial Setup
1. Once the VPS finishes provisioning, navigate to `https://[your-vps-ip]` or `https://n8n.[your-domain]`.
2. Complete the initial n8n setup wizard to create your root admin account.

---

## ⚙️ Advanced Configuration & Environment Tuning

While the one-click deployment works immediately, running a production server requires some tuning. SSH into your Hostinger VPS to modify your environment variables.

Navigate to the n8n installation directory (usually `/opt/n8n` or `/root/n8n` depending on the template structure):

```bash
cd /opt/n8n
nano .env
