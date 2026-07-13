# Self-Hosted n8n Workflow Library
This repository contains the production-ready n8n workflow JSON files for the ApexQuant automation stack, optimized for use with the Hostinger 1-click n8n VPS template.

## How to Use
1. Provision your server using the official Hostinger "Ubuntu 24.04 with n8n" template.
2. Access your n8n dashboard via your domain.
3. Download the `.json` files from the `/workflows` folder in this repository.
4. In your n8n instance, click "Import from File" and upload the JSON to instantly deploy the workflow logic.

## Workflows Included
*   **LinkedIn Automation:** Automates connection requests and profile data retrieval (built to work with official LinkedIn API capabilities).
*   **WhatsApp Chatbot:** Document-augmented chat workflow for query management.

## Requirements
*   A Hostinger VPS with the official n8n template pre-installed.
*   n8n Community Edition (included with the template).
