# Production-Grade n8n on Hostinger: Self-Hosted Automation at $6/Month

Replace your $400+/month Zapier/Make bill with enterprise-grade automation running on a **$6-12/month Hostinger KVM VPS**.

This repository contains the battle-tested Docker Compose architecture for deploying self-hosted n8n—complete with persistent PostgreSQL, reverse proxy SSL termination, and automatic health recovery. Zero per-task execution fees. Full data sovereignty. Infinite scaling.

---

## 🚀 Why Hostinger + n8n?

### The Problem
Automation platforms charge per task execution:
- **Zapier:** $0.99–$4.99 per 100 tasks → $300–500/month at scale
- **Make:** $0.10–$0.60 per operation → $200–400/month with webhook loops
- **Dependency risk:** Platform downtime = your entire pipeline stops

### The Solution
Run automation on **your own VPS**:
- **Hostinger KVM VPS:** $6–12/month (Ubuntu 24.04 LTS, 1–2 vCPU, 2–4GB RAM)
- **n8n Community Edition:** Free, open-source, self-hosted
- **Cost savings:** 98% reduction ($400 → $6/month)
- **Uptime control:** Your infrastructure, your reliability
- **Zero overage fees:** Run 1,000 workflows or 1 million—cost stays flat

---

## 📋 What You Get

✅ **Production-grade Docker Compose stack**  
✅ **Automatic SSL/TLS via Traefik reverse proxy**  
✅ **PostgreSQL 16 with persistent data volumes**  
✅ **Health checks & auto-restart on failure**  
✅ **Environment-based configuration** (no hardcoding secrets)  
✅ **Ready-to-clone deployment in 5 minutes**  

---

## 🛠️ Prerequisites

Before deploying, you'll need:

1. **A Hostinger KVM VPS** (minimum: Ubuntu 24.04 LTS, 1 vCPU, 2GB RAM)
   - [Sign up for Hostinger](https://www.hostinger.com/vps-hosting) and grab a VPS instance
   
2. **A custom domain** (e.g., `automation.yourdomain.com`)
   - You'll point this via DNS A Record to your VPS IP
   
3. **SSH access to your server**
   - Most terminal apps work: Terminal (Mac/Linux), PuTTY (Windows), or VS Code SSH

4. **Docker & Docker Compose pre-installed** (we'll install in Step 1)

---

## ⚡ Quick Start (5 Minutes)

### Step 1: Provision Your Hostinger VPS

1. Log into [Hostinger Dashboard](https://hpanel.hostinger.com)
2. Create a new **KVM VPS** → Choose **Ubuntu 24.04 LTS**
3. Once live, copy your **VPS IP address** (e.g., `185.123.456.789`)

### Step 2: Point Your Domain to the VPS

1. Go to your domain DNS settings (wherever you registered it)
2. Create an **A Record**:
   - **Host:** `automation` (or your subdomain name)
   - **Points to:** Your Hostinger VPS IP
   - **TTL:** 3600 (or default)
3. Wait 5–10 minutes for DNS propagation

### Step 3: SSH Into Your Server & Deploy

```bash
# Connect to your VPS
ssh root@YOUR_HOSTINGER_VPS_IP

# Update system packages
sudo apt-get update && sudo apt-get upgrade -y

# Install Docker & Docker Compose
sudo apt-get install -y docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker

# Clone this repository
git clone https://github.com/Nefro-stack/self-hosted-n8n-hostinger.git
cd self-hosted-n8n-hostinger

# Copy the example env file and configure it
cp .env.example .env
nano .env
```

### Step 4: Configure Your Environment

Edit `.env` and fill in these critical values:

```bash
# n8n Web UI Settings
N8N_HOST=automation.yourdomain.com          # Your custom domain
N8N_PORT=3000                               # Keep as is (internal port)
N8N_PROTOCOL=https                          # Always HTTPS

# PostgreSQL Database
DB_POSTGRE_USER=n8n                         # Database username
DB_POSTGRE_PASSWORD=YOUR_SECURE_PASSWORD    # Generate a strong password
DB_POSTGRE_DB=n8n                           # Database name

# Optional: n8n Admin Setup (set on first run)
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin@yourdomain.com
N8N_BASIC_AUTH_PASSWORD=YOUR_ADMIN_PASSWORD
```

**🔒 Security tip:** Use a password manager to generate strong passwords (16+ chars, mixed case + numbers + symbols).

### Step 5: Deploy the Stack

```bash
# Start all services in the background
docker-compose up -d

# Watch the logs (Ctrl+C to exit)
docker-compose logs -f n8n

# Check service status
docker-compose ps
```

You should see:
```
NAME          STATUS              PORTS
n8n           Up 2 minutes        0.0.0.0:3000->3000/tcp
postgres      Up 2 minutes        0.0.0.0:5432->5432/tcp
```

### Step 6: Access Your n8n Instance

1. Open your browser to: `https://automation.yourdomain.com`
2. Log in with the credentials you set in `.env`
3. Start building workflows 🚀

---

## 📁 Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│ Your Custom Domain (automation.yourdomain.com)           │
└──────────────────┬──────────────────────────────────────┘
                   │ HTTPS (Port 443)
                   ▼
        ┌──────────────────────┐
        │  Traefik Proxy       │ (Automatic SSL/TLS)
        │  (Reverse Proxy)     │
        └──────────┬───────────┘
                   │ HTTP (Port 3000)
        ┌──────────▼───────────┐
        │   n8n Container      │ (Automation Engine)
        │ ├─ n8n Core Process  │
        │ └─ Webhook Listener  │
        └──────────┬───────────┘
                   │ TCP (Port 5432)
        ┌──────────▼────────────┐
        │ PostgreSQL Container  │ (Data Persistence)
        │ ├─ Workflows          │
        │ ├─ Execution History  │
        │ └─ Credentials        │
        └───────────────────────┘

(All running on a single $6/mo Hostinger VPS)
```

---

## 🔧 Common Operations

### View Logs
```bash
# All services
docker-compose logs -f

# Just n8n
docker-compose logs -f n8n

# Just PostgreSQL
docker-compose logs -f postgres
```

### Restart a Service
```bash
docker-compose restart n8n
```

### Stop Everything
```bash
docker-compose stop
```

### Start Again
```bash
docker-compose start
```

### Reset (⚠️ deletes workflows & data)
```bash
docker-compose down -v
docker-compose up -d
```

---

## 🛡️ Monitoring & Maintenance

### Health Checks
Docker Compose includes automatic health checks. If a container crashes, it auto-restarts:
```bash
# Check service health status
docker-compose ps
```

### Database Backups
Your PostgreSQL data is persisted in a Docker volume (`postgres_data`). To back it up:

```bash
# Export the database
docker-compose exec postgres pg_dump -U n8n n8n > backup_$(date +%Y%m%d).sql

# Restore from backup (if needed)
docker-compose exec -T postgres psql -U n8n n8n < backup_20260101.sql
```

### Update n8n
To upgrade to the latest n8n version:
```bash
# Pull latest images
docker-compose pull

# Restart with new version
docker-compose up -d
```

---

## 📊 Real-World Cost Comparison

| Platform | Setup | Monthly Cost | Workflows | Per-Task Fee |
|----------|-------|--------------|-----------|--------------|
| **Zapier** | Free | $480–840 | 10–20 | $0.99–4.99 per 100 |
| **Make** | Free | $300–600 | 15–30 | $0.10–0.60 per op |
| **This Setup** | ~15 min | $6–12 | **Unlimited** | **$0** |

**Example:** Running 50 daily webhooks with 200 tasks each:
- Zapier: 10,000 tasks/month = **$500/month**
- Make: 10,000 ops/month = **$60–300/month**
- **Hostinger + n8n: $6/month** ✅

---

## 🐛 Troubleshooting

### "Connection refused" when accessing the domain
**Solution:** Wait 10–15 minutes for DNS propagation. Check with:
```bash
nslookup automation.yourdomain.com
```

### "PostgreSQL connection error"
**Solution:** Check that the DB password in `.env` matches what Docker has. Restart:
```bash
docker-compose restart postgres n8n
```

### "n8n won't start / keeps restarting"
**Solution:** Check logs:
```bash
docker-compose logs n8n
```
Common issues:
- Wrong `N8N_HOST` (must match your domain)
- PostgreSQL not ready yet (give it 30 seconds)
- Port 3000 already in use (check: `sudo lsof -i :3000`)

### "Out of disk space"
**Solution:** Docker volumes can grow large. Check usage:
```bash
docker system df

# Clean up unused images/volumes
docker system prune -a --volumes
```

---

## 📚 Learn More

- **n8n Official Docs:** https://docs.n8n.io/
- **Hostinger VPS Guide:** https://support.hostinger.com/en/articles/360005568474
- **Docker Compose Reference:** https://docs.docker.com/compose/compose-file/

---

## 🔗 Read the Full Blueprint

For the complete technical deep-dive—including workflow architecture, security hardening, and scaling patterns—read the full article on ApexQuant:

📖 **[Self-Hosted AI Automation at Scale: The Hostinger n8n Blueprint](https://apexquant.substack.com)**

---

## 📄 License

MIT License – See [LICENSE](./LICENSE) for details.

---

## 💬 Questions?

Issues? Feature requests? Create a GitHub issue or reach out:
- **GitHub Issues:** https://github.com/Nefro-stack/self-hosted-n8n-hostinger/issues
- **ApexQuant:** https://apexquant.substack.com

---

**Last updated:** July 2026  
**n8n Version:** Latest Community Edition  
**Tested on:** Hostinger KVM VPS (Ubuntu 24.04 LTS)
