# Self-Hosted n8n Workflow Templates (Hostinger VPS Optimized)

This repository contains production-ready, cloneable n8n workflow templates designed to run on a flat-cost, self-hosted Hostinger VPS. By migrating your heavy AI and outreach pipelines off managed platforms (like Zapier, Make, or n8n Cloud), you can run unlimited executions without usage-metered billing.

## 🚀 Infrastructure Setup

Instead of manually configuring Docker, Traefik, PostgreSQL, and SSL certs via the terminal, this architecture uses Hostinger's pre-configured **Ubuntu 24.04 with n8n** VPS template.

1. Pick a KVM plan on [Hostinger VPS](https://www.hostinger.com/self-hosted-n8n).
2. Select the **Ubuntu 24.04 with n8n** template during setup.
3. Access your instance at `https://n8n.[your-vps-hostname]` and complete the admin setup wizard.

For advanced configurations, environment variables, or queue-mode scaling, refer to the [Official Hostinger n8n Template Guide](https://www.hostinger.com/support/10473267-how-to-use-the-n8n-vps-template-at-hostinger/).

---

## 📂 Included Workflow Templates

To import these workflows directly into your self-hosted n8n canvas, download the JSON files from this repository:

### 1. WhatsApp AI Chatbot with Text, Image, & PDF Automation (`whatsapp-ai-chatbot-text-image-pdf-automation.json`)
An advanced, multi-modal AI customer support flow capable of handling inbound text messages, processing user-uploaded images/PDFs, and returning context-aware AI responses via WhatsApp.
* **Nodes used:** Webhook, Advanced AI (OpenAI/Gemini), Code Node (Binary data parsing for PDFs/Images), WhatsApp Business Platform.
* **Production Note:** The WhatsApp Business Platform requires an official Meta Developer Account, business verification, and pre-approved message templates. 

### 2. B2B Cold Outreach Email Generator (`B2B Cold Outreach Email Generator (2).json`)
A high-volume outreach engine that automatically researches target companies, drafts hyper-personalized B2B cold emails using LLMs, and queues them for delivery.
* **Nodes used:** HTTP Request (Data Enrichment/Scraping), OpenAI/Gemini (Personalization Engine), Switch Node, Email/SMTP/Resend Node.
* **Production Note:** Ensure you connect this workflow to a verified outreach infrastructure (like Resend, SendGrid, or a dedicated Google Workspace SMTP) with proper SPF/DKIM records to avoid spam folders.

---

## 📥 How to Import to n8n

1. Download the `.json` workflow file you want to use from this repository.
2. Open your self-hosted n8n dashboard.
3. Click **Create workflow** in the top right.
4. Click the **three dots (menu)** in the top right of the canvas.
5. Select **Import from file** and upload the downloaded JSON.
6. Set your API credentials for the nodes (Meta, OpenAI/Gemini, Email providers) and toggle the workflow to **Active**.

---

*Maintained by Pratik Batha at [ApexQuant](https://apexquant.substack.com).*
