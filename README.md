# Self-Hosted n8n on Hostinger VPS - Production Blueprints

Welcome to the official repository for the **ApexQuant** self-hosted n8n infrastructure. This repo contains the raw JSON blueprints and setup references for running n8n Community Edition in production on a Hostinger VPS.

## 🚀 The Infrastructure

We replaced expensive cloud automation platforms with a scalable, self-hosted node. 

- **Hosting Provider:** Hostinger (KVM1 or KVM2 Plan)
- **OS/Template:** Ubuntu 24.04 with n8n (One-click deployment)
- **Included Stack:** n8n Community Edition, Docker, PostgreSQL, Traefik, Let's Encrypt SSL.

## 🛠️ Quick Start Guide

1. **Provision:** Get a Hostinger VPS and select the `Ubuntu 24.04 with n8n` template during checkout.
2. **Setup:** Navigate to `https://n8n.[your-domain]` immediately after the server boots to complete the setup wizard.
3. **Import:** In your n8n dashboard, click **Import from File**, select any JSON file from this repository, and activate your workflow.

## 📈 Scaling

If your task execution volume (e.g., 50,000+ tasks/month) begins to bottleneck your KVM1 or KVM2 plan, you can vertically scale up to KVM4 directly from your Hostinger dashboard without needing to migrate your database or rebuild networks.

---
*Created by Patrick Blaze for the ApexQuant community. Join 22,000+ founders scaling their operations with AI at [ApexQuant](https://apexquant.substack.com/).*
