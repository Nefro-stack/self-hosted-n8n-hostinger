# Self-Hosted n8n Workflow Templates (Hostinger VPS Optimized)

This repository contains production-ready, cloneable n8n workflow templates designed to run on a flat-cost, self-hosted Hostinger VPS. By migrating your workflows off managed platforms (like Zapier, Make, or n8n Cloud), you can run unlimited executions without usage-metered billing.

## 🚀 Infrastructure Setup

Instead of manually configuring Docker, Traefik, PostgreSQL, and SSL certs via the terminal, this guide uses Hostinger's pre-configured **Ubuntu 24.04 with n8n** VPS template.

1. Pick a KVM plan on [Hostinger VPS](https://www.hostinger.com/self-hosted-n8n).
2. Select the **Ubuntu 24.04 with n8n** template during setup.
3. Access your instance at `https://n8n.[your-vps-hostname]` and complete the admin setup wizard.

For advanced configurations, environment variables, or queue-mode scaling, refer to the [Official Hostinger n8n Template Guide](https://www.hostinger.com/support/10473267-how-to-use-the-n8n-vps-template-at-hostinger/).

---

## 📂 Included Workflow Templates

To import these workflows directly into your self-hosted n8n canvas, download the JSON files from this repository:

### 1. Inbound Lead Capture & Enrichment (`inbound-lead-capture.json`)
A production-ready pipeline for capturing incoming leads via webhook, deduping them against a local database, and routing notifications.
* **Nodes used:** Webhook, HTTP Request (Enrichment), PostgreSQL (Deduplication), Slack/Email notifications.
* **How to use:** Import the JSON file, configure your PostgreSQL credentials, and add your webhook URL to your landing page form.

### 2. Document-Augmented WhatsApp Support Assistant (`whatsapp-support-assistant.json`)
An AI-powered customer support flow that references internal documents to answer questions via WhatsApp.
* **Nodes used:** Webhook, OpenAi/Gemini LLM, Document Parser, WhatsApp Business.
* **Note on RAG:** To scale this into a true semantic vector search (RAG) system, connect the document parser node to an external vector database (such as Qdrant or Pinecone), or enable the `pgvector` extension on your VPS's native PostgreSQL instance.
* **Note on WhatsApp:** This workflow requires a verified Meta Business Account and pre-approved WhatsApp message templates.

---

## 📥 How to Import to n8n

1. Download the `.json` workflow file you want to use from this repository.
2. Open your self-hosted n8n dashboard.
3. Click **Create workflow** in the top right.
4. Click the **three dots (menu)** in the top right of the canvas.
5. Select **Import from file** and upload the downloaded JSON.
6. Set your credentials for the nodes (databases, APIs, Slack) and toggle the workflow to **Active**.

---

*Maintained by Pratik Batha at [ApexQuant](https://apexquant.substack.com).*
