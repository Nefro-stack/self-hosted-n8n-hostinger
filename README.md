# Self-Hosted n8n on Hostinger VPS

Production-grade workflow automation for $5/month instead of $400/month.

> **Read the full technical guide:** [Replace Your $400/Month Cloud Automation Bill](https://apexquant.substack.com/p/hostinger-n8n-blueprint)

---

## What This Does

This repository contains everything needed to deploy an enterprise-grade, self-hosted n8n instance on a Hostinger KVM2 VPS (or any Ubuntu 24.04 server).

**What you get:**
- ✅ n8n workflow orchestration (unlimited executions, full enterprise features)
- ✅ PostgreSQL persistent database (disaster recovery ready)
- ✅ Traefik reverse proxy with automatic HTTPS/TLS
- ✅ Redis caching for performance scaling
- ✅ Docker Compose orchestration (reproducible, version-controlled)
- ✅ Backup & restoration scripts (automated daily backups)
- ✅ Production security hardening (JWT, encryption keys, firewall rules)

**Cost comparison:**
| Platform | Monthly Cost | Annual Cost | Execution Limit |
|----------|------|------|------|
| Zapier Professional | $299 | $3,588 | 1,500 tasks |
| Make Enterprise | $500+ | $6,000+ | Custom |
| Hosted n8n | $500+ | $6,000+ | Metered |
| **Your Hostinger VPS** | **$5–15** | **$60–180** | **Unlimited** |

---

## Quick Start (5 minutes)

### Prerequisites
- Hostinger KVM2 VPS (or any Ubuntu 24.04 instance)
- SSH access to the server
- `docker` and `docker-compose` installed

### Deploy

```bash
# 1. SSH into your server
ssh root@YOUR_VPS_IP

# 2. Clone this repository
cd /home
git clone https://github.com/Nefro-stack/self-hosted-n8n-hostinger.git
cd self-hosted-n8n-hostinger

# 3. Configure your environment
cp .env.example .env
nano .env  # Edit with your settings

# 4. Start the stack
docker compose up -d

# 5. Verify everything is running
docker compose ps
```

**Within 45 seconds, n8n is running at:**
- `http://YOUR_VPS_IP:5678` (if using IP)
- `https://your-domain.com` (if domain is configured)

---

## Directory Structure

```
self-hosted-n8n-hostinger/
├── docker-compose.yml           # Complete service orchestration
├── .env.example                 # Configuration template (copy to .env)
├── README.md                    # This file
│
├── nginx/
│   └── nginx.conf               # Reverse proxy configuration
│                                # (Optional: Traefik alternative)
│
├── traefik/
│   ├── traefik.yml              # Traefik core configuration
│   └── dynamic.yml              # Dynamic routing rules
│
├── scripts/
│   ├── init.sh                  # Database initialization
│   ├── backup.sh                # Automated backup routine
│   ├── restore.sh               # Point-in-time restoration
│   └── health-check.sh          # System health verification
│
└── docs/
    ├── SECURITY.md              # Security hardening guide
    ├── TROUBLESHOOTING.md       # Common issues & solutions
    └── SCALING.md               # Performance optimization
```

---

## Configuration (.env)

Copy `.env.example` to `.env` and update these critical variables:

```env
# Database
DB_POSTGRESDB_PASSWORD=your_strong_password_here
POSTGRES_PASSWORD=your_strong_password_here

# n8n Core
N8N_HOST=workflows.youragency.com      # Your domain or VPS IP
N8N_PORT=5678
N8N_PROTOCOL=https                     # Use HTTPS in production

# Security Keys (Generate with: openssl rand -base64 32)
ENCRYPTION_KEY=your_32_character_key_here
JWT_SECRET=your_32_character_secret_here

# Storage
BACKUP_PATH=/backups
N8N_USER_FOLDER=/n8n_data

# Email Notifications (Optional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=app_password_here
```

**Never commit `.env` to version control.** Keep it local or in a secrets manager.

---

## Services Included

### n8n (Port 5678)
Workflow orchestration engine. Runs workflows on schedule or via webhook. Full enterprise feature set: API access, workflow versioning, credential management, execution history.

### PostgreSQL (Port 5432, internal only)
Persistent relational database. Stores workflow definitions, execution history, credentials, and user data. Automatically initialized on first run.

### Traefik (Port 80/443)
Reverse proxy with automatic HTTPS certificate provisioning via Let's Encrypt. Handles domain routing and SSL/TLS termination.

### Redis (Port 6379, internal only)
In-memory cache for performance scaling. Reduces database load on high-frequency workflows.

---

## First Time Setup

### Step 1: Initialize the Database

```bash
docker compose exec postgres psql -U n8n -d n8n_db -c "\dt"
```

This lists all n8n tables. If you see `execution`, `workflow`, and `credentials`, initialization succeeded.

### Step 2: Access n8n

Open your browser:
- **Local testing:** `http://YOUR_VPS_IP:5678`
- **Production:** `https://your-domain.com`

### Step 3: Create Owner Account

Fill in the setup form:
- Email: Your operations email
- First Name: Your name or organization
- Password: Strong password (8+ chars, uppercase, number)

### Step 4: Import Your First Workflow

Example: LinkedIn DM automation, WhatsApp RAG chatbot, Airtable sync, or any custom workflow.

---

## Backup & Disaster Recovery

### Automated Daily Backups

Edit crontab:
```bash
crontab -e
```

Add this line to backup at 2 AM daily:
```
0 2 * * * /home/self-hosted-n8n-hostinger/scripts/backup.sh
```

### Manual Backup

```bash
./scripts/backup.sh
```

Creates timestamped backup at `/backups/n8n_backup_YYYYMMDD_HHMMSS.sql.gz`

### Restore from Backup

```bash
./scripts/restore.sh /backups/n8n_backup_20260608_020000.sql.gz
```

This overwrites your current database with the backup. Use with caution on production.

---

## Security Hardening

### Firewall Rules

Allow only essential ports:
```bash
sudo ufw default deny incoming
sudo ufw allow 22/tcp     # SSH
sudo ufw allow 80/tcp     # HTTP (for Let's Encrypt)
sudo ufw allow 443/tcp    # HTTPS
sudo ufw enable
```

### SSH Key Authentication

Disable password authentication:
```bash
sudo sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### Encrypt Sensitive Data

The `.env` file contains secrets. Protect it:
```bash
chmod 600 .env
```

### Regular Updates

Keep containers updated:
```bash
docker compose pull
docker compose down
docker compose up -d
```

---

## Monitoring & Health Checks

### Check All Services Running

```bash
docker compose ps
```

Expected output:
```
NAME         IMAGE              STATUS
n8n          n8nio/n8n:latest   Up 2 minutes
postgres     postgres:15-alpine Up 2 minutes
traefik      traefik:v2.10      Up 2 minutes
redis        redis:7-alpine     Up 2 minutes
```

### Monitor Resource Usage

```bash
docker stats
```

Typical healthy usage:
- n8n: 200–400 MB RAM
- PostgreSQL: 300–500 MB RAM
- Traefik: 50–100 MB RAM
- Total: <1 GB RAM

### View Live Logs

```bash
docker compose logs -f n8n
```

---

## Scaling to Production

### Increase Resource Allocation

If workflows are slow, upgrade your VPS plan:

```bash
# Hostinger KVM2 Basic  → Standard: adds 2 vCPU + 2GB RAM
# Hostinger KVM2 Standard → Premium: adds 4 vCPU + 4GB RAM
```

No downtime required. Containers automatically use additional resources.

### Load Testing

Before going live with high-volume workflows:

```bash
docker run --rm loadimpact/k6 run - < load_test.js
```

This simulates 100 concurrent webhook executions. Verify n8n handles the load without errors.

### Database Optimization

For workflows with 100,000+ executions:

```bash
docker compose exec postgres psql -U n8n -d n8n_db -c "
CREATE INDEX idx_execution_workflow ON execution(workflow_id);
CREATE INDEX idx_execution_status ON execution(status);
VACUUM ANALYZE;
"
```

This optimizes query performance on high-volume execution history.

---

## Troubleshooting

### n8n Won't Start

```bash
docker compose logs n8n
```

Usually means PostgreSQL isn't ready. Wait 10 seconds and retry:
```bash
docker compose restart n8n
```

### SSL Certificate Error

If domain isn't resolving to your VPS IP, Traefik can't provision a cert. Verify DNS:

```bash
nslookup your-domain.com
```

Should return your VPS IP. Give DNS 5–10 minutes to propagate after pointing your domain.

### Database Connection Failed

Check PostgreSQL logs:
```bash
docker compose logs postgres
```

Most common issue: `DB_POSTGRESDB_PASSWORD` in `.env` doesn't match `POSTGRES_PASSWORD`. Must be identical.

### Workflows Are Slow

Check which container is consuming resources:
```bash
docker stats
```

If PostgreSQL is at >80% CPU:
- Your workflow query is inefficient (optimize the SQL)
- Add indexes to frequently-queried tables

If n8n is at >90% CPU:
- Your workflow has infinite loops or very high-frequency triggers
- Reduce execution frequency or optimize loop logic

---

## Advanced Usage

### Custom Domains

Update `.env`:
```env
N8N_HOST=workflows.youragency.com
```

Traefik automatically provisions a Let's Encrypt certificate.

### Multiple n8n Instances

Deploy separate instances for different teams:
```bash
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

Requires separate `.env` files for each instance.

### Integration Examples

Ready-to-use workflows for common integrations:
- LinkedIn API (DM automation, profile scraping)
- WhatsApp Business API (chatbots, notifications)
- Airtable (data sync, automation)
- Salesforce (CRM integration)
- Google Sheets (data input/output)

See `/examples/workflows/` for starter templates.

---

## Contributing

Found a bug? Want to optimize the setup? Open a PR or issue.

This repository is maintained and tested on real Hostinger infrastructure.

---

## License

MIT License. Use freely. Attribution appreciated but not required.

---

## Support & Resources

- **Full Technical Guide:** https://apexquant.substack.com/p/hostinger-n8n-blueprint
- **n8n Docs:** https://docs.n8n.io
- **Docker Docs:** https://docs.docker.com
- **Hostinger VPS Docs:** https://support.hostinger.com/en/articles/categories/7602373-vps

---

## Cost Comparison (Real Math)

**First Year:**
- Hostinger VPS: $60 (basic plan, $5/month)
- Domain: $12 (register or transfer)
- Backups (included): Free
- **Total: $72**

**Equivalent Setup on Zapier:**
- Professional plan (1,500 tasks): $299/month = $3,588/year
- **Total: $3,588**

**Savings in Year 1:** $3,516  
**Savings in Year 2+:** $3,516/year (perpetual)

After running just 3 complex workflows, your self-hosted instance has paid for itself.

---

## Author

**Pratik Batha (Patrick Blaze)**  
CEO, Kynlyr LLC  
Founder, ApexQuant  

- GitHub: [@Nefro-stack](https://github.com/Nefro-stack)
- Substack: [ApexQuant](https://apexquant.substack.com)
- LinkedIn: [Patrick Blaze](https://www.linkedin.com/in/patrick-blaze-873548390)

---

**Last Updated:** June 2026  
**Tested On:** Hostinger KVM2, Ubuntu 24.04 LTS, Docker 27.0+  
**Status:** Production-Ready ✅

Made for 22,000+ AI builders, agency owners, and technical founders.
