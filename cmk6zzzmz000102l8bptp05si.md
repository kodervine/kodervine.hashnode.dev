---
title: "Unlimited Automation for $5. Why I Moved to n8n on Contabo"
datePublished: Fri Jan 09 2026 14:55:19 GMT+0000 (Coordinated Universal Time)
cuid: cmk6zzzmz000102l8bptp05si
slug: unlimited-automation-for-5-why-i-moved-to-n8n-on-contabo
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1767970152604/791e3292-d407-48bf-a29a-f2081ed307e6.png
tags: cloud-computing, automation, self-hosted, n8n, contabo

---

Most people start automation with tools like Zapier or Make. These "Managed Cloud" platforms are perfect for SMEs and non-technical teams who need to move fast without worrying about server maintenance or security patches. They offer "peace of mind" by outsourcing infrastructure risks to third-party specialists.

However, they are great until you get the bill. I needed something that didn't charge me for every single "task" and, more importantly, something that kept clients' data under my own lock and key.

Beyond the benefits, I decided to self-host to improve my DevOps skills. While cloud platforms handle the "hard stuff" for you, they also hide the internal mechanics of how software really runs. By self-hosting, I forced myself to learn networking, Docker, and system administration which are the exact skills that separate a "coder" from a "cloud infrastructure engineer".

## Why n8n?

8n is an open-source automation tool that you "own".

* **The Cost Cap**: Managed tools often charge by the minute or task, leading to surprise bills. Since it's on my server, I pay a flat monthly fee and can run 1,000 or 1,000,000 tasks for the same price.
    
* **AI Ready**: It has built-in LangChain nodes, which is a big deal if you want to build AI agents that actually "think" and process data independently.
    
* **Privacy**: In cybersecurity, sending sensitive logs to a third party is a risk. With n8n, the credentials and deployment logs stay entirely within my infrastructure.
    

## Why Contabo?

I looked at a lot of providers. Contabo won because they are the "RAM kings". For about $5 a month, I got 8GB of RAM. Most other providers give you 2GB for that price. When you start running AI workflows, that extra RAM is what keeps your server from crashing.

## The Step-by-Step Build

### 1\. Connecting Your Domain: DNS Step

**What to do**: Log into your domain provider (Vercel, GoDaddy, Cloudflare, etc.) and create an **A Record**.

* **Name/Host**: `n8n` (This creates the subdomain `n8n.yourdomain.com`).
    
* **Value/Points to**: `[YOUR_CONTABO_IP]`.
    
* **TTL**: Set to `600` or `Auto`.
    

**Why this matters:**

Think of your server’s IP address like the GPS coordinates of a house in the middle of nowhere. It is hard to remember and looks unprofessional to clients. A DNS record acts like a "Contact" in your phone. Instead of typing the coordinates, you just type the name "yoursubdomain.yourdomain.com," and the internet automatically routes you to the right server.

**The "Propogation" Wait:**

After you save this, the change doesn't happen instantly. The internet is a massive network of address books that all need to update. Usually, it takes 5 to 10 minutes, but it can take up to 24 hours. You can check if yours is live by visiting whatsmydns.net and typing in your subdomain.

### Pro-Tip: Vercel vs. Others

Since I use **Vercel**, the process is incredibly fast because their "Global Edge Network" updates records almost instantly. If you are using a traditional registrar like GoDaddy, you might notice a slightly longer wait before your server "responds" to the domain name.

### 2\. Installing the Docker Engine

What to do: Run the Docker install script on your Ubuntu server.

Why: Think of Docker as a "shipping container" for software. It packages n8n with everything it needs to run, so you don't have to worry about missing files or conflicting software on your server.

Bash

```plaintext
# This script automatically handles the installation parts
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### 3\. Permissions & Folders

What to do: Create a folder for n8n and set the ownership to 1000:1000.

Why: By default, Linux is very strict about who can write files. n8n runs as a specific "user" inside its container (ID 1000). If you don't give that ID permission, n8n won't be able to save your workflows.

Bash

```bash
mkdir ~/n8n && cd ~/n8n
mkdir n8n_data
sudo chown -R 1000:1000 n8n_data
```

### 4\. Docker Compose

What to do: Create a docker-compose.yml file and paste the configuration.

Why: This file is basically a set of instructions for the server. It tells Docker which version of n8n to download, which port to use (5678), and where to store your secret encryption key.

YAML

```plaintext
# Simplified Blueprint
services:
  n8n:
    image: n8nio/n8n:latest
    restart: always # This ensures n8n starts back up if the server reboots
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=yoursubdomain.yourdomain.com
      - N8N_PROTOCOL=https
      - N8N_ENCRYPTION_KEY=[REDACTED]
    volumes:
      - ./n8n_data:/home/node/.n8n
```

### 5\. Nginx: The Gatekeeper

What to do: Install Nginx and set up a reverse proxy.

Why: Docker is running n8n on `port 5678`, but the internet usually talks on ports 80 (HTTP) and 443 (HTTPS). Nginx sits at the "gate" and redirects anyone visiting your domain to the right place inside your server.

### 6\. SSL Security (The Padlock)

What to do: Run Certbot to get a free SSL certificate.

Why: You should never log into a dashboard over an unencrypted connection, especially in cybersecurity. This step adds the "HTTPS" padlock to your URL so your login info stays private.

Bash

```plaintext
sudo certbot --nginx -d yoursubdomain.yourdomain.com
```

Once, everything is successful, the link to your live site should show on your terminal

## Final Thoughts

Self-hosting enables you to have total control over your tech stack. Now, I have a highly scalable automation engine that costs less than a fancy cup of coffee per month can import my workflows to the new account

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1767869186475/556875ab-5366-4f57-ad8b-be538ce7fe3e.png align="center")