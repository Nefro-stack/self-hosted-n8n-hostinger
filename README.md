# Self-Hosted n8n Architecture & Workflow Blueprints (Hostinger VPS)

[![Substack Article](https://img.shields.io/badge/ApexQuant-Substack_Deep--Dive-FF6700?style=for-the-badge&logo=substack)](https://apexquant.substack.com/p/d8122c0f-3e26-459f-8606-e2c849ead443)
[![Hostinger VPS](https://img.shields.io/badge/Infrastructure-Hostinger_KVM2_VPS-673DE6?style=for-the-badge&logo=hostinger)](https://www.hostg.xyz/aff_c?offer_id=48&aff_id=239394&url_id=4660)
[![n8n Community](https://img.shields.io/badge/Orchestration-n8n_Community-EA4B71?style=for-the-badge&logo=n8n)](https://n8n.io)

This repository contains the production-ready n8n workflow blueprints and deployment architecture featured in the [ApexQuant Substack guide](https://apexquant.substack.com/p/d8122c0f-3e26-459f-8606-e2c849ead443): **"I Replaced an Expensive Make.com Bill with a $8.79/mo Self-Hosted n8n on Hostinger VPS"**.

---

## 🚀 Quick Start: Deployment on Hostinger VPS

Rather than manually installing Docker, Traefik, or PostgreSQL via SSH, this setup utilizes Hostinger's pre-configured **Ubuntu 24.04 with n8n** template.

### 1. Provision Your Server
Deploy a [Hostinger KVM2 VPS ($8.79/mo)](https://www.hostg.xyz/aff_c?offer_id=48&aff_id=239394&url_id=4660) selecting the **n8n** operating system template during checkout. 

> 💡 **Tip:** Choosing a 12-month or 24-month plan includes a free custom domain for the first year, providing the best long-term value for production environments.

### 2. Initial Setup & Access
1. Once provisioned, complete the setup wizard in your Hostinger hPanel.
2. Navigate to `https://n8n.[your-domain].com` to set up your primary admin account.
3. Your stack (n8n, PostgreSQL, Traefik SSL reverse proxy) runs out of the box.

### 3. Maintenance & Version Updates
Updating your n8n instance requires zero terminal commands. Navigate to **Docker Manager** inside your Hostinger hPanel and click **Update** to pull and deploy the latest official image seamlessly.

---

## 📦 Included Workflow Blueprints

You can find the raw `.json` workflow files in the repository root. Import them directly into your n8n canvas via `Workflows` -> `Import from File`.

### 1. B2B Cold Outreach Generator (`B2B Cold Outreach Email Generator (2).json`)
* **Core Engine:** Analyzes incoming webhook leads using Groq (Llama 3.3).
* **Logic Flow:** Filters low-fit prospects via a `Switch` node and generates personalized draft emails directly inside Gmail for team review.
* **Production Note:** Always use dedicated outbound email infrastructure (Resend, SendGrid, Mailgun) on secondary domains with properly configured SPF/DKIM/DMARC records for high-volume delivery.

### 2. WhatsApp Multi-Modal AI Support Agent (`whatsapp-ai-chatbot-text-image-pdf-automation.json`)
* **Core Engine:** Receives inbound customer messages via WhatsApp Webhooks and processes text, receipts, and PDF binary attachments.
* **Logic Flow:** Generates context-aware responses using multi-modal LLM nodes and formats replies back to the user.
* **Production Note:** Requires an official Meta Developer account and WhatsApp Business verification. For document retrieval (RAG), connect an external vector database (e.g., Pinecone or Qdrant) as native pgvector is not pre-installed on standard PostgreSQL templates.

---

## ⚡ Unit Economics Comparison

| Platform | Monthly Cost (50,000 Ops/Mo) | Scaling Model |
| :--- | :--- | :--- |
| **Zapier (Pro)** | ~$299/mo | Per task action |
| **n8n Cloud (Pro)** | ~$150–$200/mo | Per execution pack |
| **Make.com (Core)** | ~$78/mo | Per operation credit |
| **Hostinger VPS (KVM2)** | **$8.79/mo** | **Flat resource cost (Unlimited Ops)** |

---

## 🔗 Resources & References

* **Deploy Hostinger VPS:** [Get Hostinger KVM2 ($8.79/mo)](https://www.hostg.xyz/aff_c?offer_id=48&aff_id=239394&url_id=4660)
* **Read the Full Architectural Guide:** [ApexQuant Substack Blueprint](https://apexquant.substack.com/p/d8122c0f-3e26-459f-8606-e2c849ead443)
* **Official Hostinger Template Docs:** [Hostinger VPS n8n Setup](https://www.hostg.xyz/aff_c?offer_id=48&aff_id=239394&url_id=4660)

---

## ⚖️ License
Distributed under the MIT License. Built for agency owners and technical founders by [ApexQuant](https://apexquant.substack.com).
