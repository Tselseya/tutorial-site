---
title: "Self-Hosted AI Stack Setup Guide"
date: 2026-05-10
description: "Complete walkthrough for installing Docker, running the n8n self-hosted AI starter kit, and setting up ngrok inside Docker"
tags: [docker, n8n, ngrok, self-hosted, tutorial]
---

# Self-Hosted AI Stack Setup Guide

A complete walkthrough for installing Docker, running the n8n self-hosted AI starter kit, and setting up ngrok inside Docker so your webhooks stay live without keeping a terminal open. Written for both first-timers and future-you who forgot how this works.

**Tags:** Beginner-Friendly · Windows · Mac · Linux · Docker Desktop · n8n · ngrok · Ollama · Qdrant · Postgres

---

## Before You Start

You need three things set up before anything in this guide will work.

| Requirement | Why you need it | Where to get it |
|-------------|-----------------|-----------------|
| **A computer** (Windows 10+, macOS 12+, or Linux) | Docker runs the containers. You need at least 8GB RAM for the full stack. | You already have this. |
| **Git** | Used to download (clone) the n8n starter kit repo from GitHub. | [git-scm.com/downloads](https://git-scm.com/downloads) |
| **A terminal** | You'll run commands in it. PowerShell on Windows, Terminal on Mac/Linux. | Already on your system. Docker Desktop also has a built-in terminal. |
| **A text editor** | You'll edit the docker-compose.yml file. VS Code is recommended. | [code.visualstudio.com](https://code.visualstudio.com/) |

> **Tip:** Using Docker Desktop's built-in terminal
> Once Docker Desktop is installed, you can run all commands from its built-in terminal instead of opening a separate PowerShell or Terminal window. Click the **Terminal** button in the bottom-right of Docker Desktop.

---

## Part 1: Installing Docker

Docker is the engine that runs everything. It packages software into isolated containers so you don't have to manually install n8n, Postgres, or Ollama on your machine. Install Docker once, and everything else in this guide follows.

### Step 1: Download Docker Desktop

Go to [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) and download the installer for your operating system.

**Windows:** Download the **Docker Desktop for Windows** installer (.exe). Run it and follow the prompts. When asked, enable **WSL 2** (Windows Subsystem for Linux) — Docker needs this to run on Windows. Restart your computer after installation.

> **Warning:** WSL 2 is required on Windows
> If Docker asks you to install the WSL 2 kernel update, do it. Go to [aka.ms/wsl2kernel](https://aka.ms/wsl2kernel), download and run the update, then restart.

**Mac:** Download the correct version for your chip: **Apple Silicon (M1/M2/M3/M4)** or **Intel**. Open the .dmg file, drag Docker to Applications, and launch it. macOS will ask for your password to install helper components — allow it.

> **Info:** Unsure which Mac you have?
> Click the Apple menu → About This Mac. If Chip shows "Apple M..." you need the Apple Silicon build. If it shows Intel Core, download the Intel version.

**Linux:** Install Docker Engine and Docker Compose directly. On Ubuntu/Debian:

```bash
sudo apt-get update
sudo apt-get install docker.io docker-compose-v2
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

Log out and back in after the last command so your user has Docker permissions.

### Step 2: Launch Docker Desktop

Open Docker Desktop from your applications. The first launch takes 1–2 minutes while it initializes. You'll see the Docker whale icon in your taskbar (Windows) or menu bar (Mac) when it's ready.

> **Tip:** You don't need a Docker account
> Docker will prompt you to sign in or create an account. You can skip this — it's not required to run the stack in this guide.

### Step 3: Verify the Installation

Open a terminal (or Docker Desktop's built-in terminal) and run:

```bash
docker --version
docker compose version
```

You should see version numbers printed for both. If you get "command not found", Docker isn't installed correctly — restart and try the installer again.

> **Tip:** Expected output looks like this
> Docker version 27.x.x
> Docker Compose version v2.x.x

---

## Part 2: Installing the n8n Starter Kit

The self-hosted AI starter kit is a pre-configured Docker setup that runs n8n, Postgres, Ollama, and Qdrant together with a single command. You get a full local AI automation environment without configuring each service manually.

> **Info:** What's included in the stack
> - **n8n** — your automation workflow engine
> - **Postgres** — the database that stores all your workflows and credentials
> - **Ollama** — runs local AI models on your machine
> - **Qdrant** — vector database for AI memory and search

### Step 1: Clone the Repository

Open a terminal and navigate to where you want the project folder to live. Then run:

```bash
git clone https://github.com/n8n-io/self-hosted-ai-starter-kit.git
cd self-hosted-ai-starter-kit
```

This downloads the starter kit to your machine and enters the project folder. Everything from here runs inside this folder.

> **Tip:** Where to put it
> Put it somewhere easy to find. On Windows: `C:\Users\YourName\`. On Mac/Linux: `~/` (your home folder). Avoid paths with spaces in the name.

### Step 2: Start the Full Stack

From inside the `self-hosted-ai-starter-kit` folder, run:

**CPU Only (No GPU)** — Most users:

```bash
docker compose --profile cpu up -d
```

**NVIDIA GPU acceleration:**

```bash
docker compose --profile gpu-nvidia up -d
```

**AMD GPU acceleration:**

```bash
docker compose --profile gpu-amd up -d
```

The `-d` flag runs everything in the background. The first time this runs, Docker will download all the images — this takes 5–15 minutes depending on your internet speed.

> **Warning:** First run takes a while — that's normal
> Docker is downloading the n8n, Postgres, Ollama, and Qdrant images. You'll see lots of text scrolling. Wait for it to finish before proceeding.

### Step 3: Check That Everything is Running

In Docker Desktop, click **Containers** in the left sidebar. You should see a group named **self-hosted-ai-starter-kit** with these containers running (green dot):

- **n8n** — port 5678
- **postgres-1** — your workflow database
- **qdrant** — port 6333
- **ollama** — port 11434

Open your browser and go to `http://localhost:5678` — you should see the n8n setup screen.

### Step 4: Create Your n8n Account

The first time you open n8n at `localhost:5678`, it shows an account setup screen. Fill in:

- Your email address
- A password you'll remember
- Your first and last name

Click **Create account**. This creates the owner account for your self-hosted instance. After this, n8n goes straight to your workflow dashboard.

> **Warning:** Don't delete the Postgres container or its volume
> All your workflows and credentials live inside the **postgres-1** container's volume. Deleting that volume means losing everything. The n8n container itself is safe to delete and recreate — only the database volume matters.

---

## Part 3: Adding ngrok to Docker

ngrok creates a public URL that points to your local n8n. Without it, services like Instagram, WhatsApp, or any other webhook-based integration can't reach your n8n instance because it's only visible on your local network. Running ngrok inside Docker means it starts automatically with your stack — no PowerShell window needed.

### Step 1: Create a Free ngrok Account

Go to [ngrok.com](https://ngrok.com) and sign up for a free account. You need this for your authtoken and your static domain.

### Step 2: Get Your Authtoken

After signing in, go to **Getting Started → Your Authtoken** in the left sidebar. Copy the token — it's a long string that looks like:

```
2abc123XYZexample_TokenStringHere
```

> **Warning:** Keep this token private
> Your authtoken is like a password. Don't paste it into public GitHub repos or share it in screenshots. If it gets exposed, go back to the ngrok dashboard and click **Reset Authtoken** to generate a new one.

### Step 3: Get a Static Domain

Free ngrok accounts get one permanent domain. Go to **Universal Gateway → Domains** in the ngrok dashboard. Click **New Domain** — ngrok generates a free static domain for you that looks like:

```
your-word-here.ngrok-free.app
```

Copy this domain. You'll use it in the next step. This domain never changes, so your webhook URLs stay stable across restarts.

> **Tip:** Why a static domain matters
> Without a static domain, ngrok generates a random URL every time it starts. That means re-configuring every webhook in Meta, Slack, or wherever you've set them up. A static domain locks your URL permanently.

### Step 4: Edit docker-compose.yml

Open the `docker-compose.yml` file inside your `self-hosted-ai-starter-kit` folder in VS Code:

**Windows:**
```bash
code C:\Users\YourName\self-hosted-ai-starter-kit\docker-compose.yml
```

**Mac / Linux:**
```bash
code ~/self-hosted-ai-starter-kit/docker-compose.yml
```

Scroll to the very bottom of the file. At the end of the `services:` section, paste this block. Make sure the indentation matches the other services (2 spaces before `ngrok:`):

```yaml
# Add this at the bottom of the services: section
ngrok:
  image: ngrok/ngrok:latest
  restart: unless-stopped
  command: http --domain=YOUR-DOMAIN.ngrok-free.app http://n8n:5678 --log=stdout
  environment:
    - NGROK_AUTHTOKEN=YOUR_AUTHTOKEN_HERE
  networks:
    - demo
  depends_on:
    - n8n
```

Replace `YOUR-DOMAIN.ngrok-free.app` with your actual static domain, and `YOUR_AUTHTOKEN_HERE` with your actual authtoken.

> **Warning:** The network: demo line is required
> This is the most common mistake. The ngrok container needs to be on the same internal Docker network as n8n to reach it by hostname. Without `networks: - demo`, ngrok will say `no such host` and fail to connect even though both containers are running.

> **Tip:** Why http://n8n:5678 and not localhost:5678?
> Inside Docker, containers don't see each other as `localhost`. They use the container name as the hostname. Since your n8n container is named `n8n`, ngrok reaches it at `http://n8n:5678`.

### Step 5: Start the ngrok Container

Save the file, then run this in the Docker Desktop terminal (make sure you're in the starter kit folder):

```bash
cd path/to/self-hosted-ai-starter-kit
docker compose up -d ngrok
```

Docker will pull the ngrok image and start the container. In Docker Desktop, you should see **ngrok-1** appear in your container list with a green dot.

> **Tip:** How to confirm it's working
> Click on the ngrok-1 container in Docker Desktop and open its **Logs** tab. Look for a line that says `started tunnel ... url=https://your-domain.ngrok-free.app`. Then visit that URL in your browser — you should see your n8n login screen.

> **Warning:** If ngrok keeps restarting (spinning icon)
> Check the container logs. Common causes: invalid authtoken (regenerate it in the ngrok dashboard), or the token was already in use from a separate PowerShell ngrok session running at the same time. Close any other ngrok instances before starting the Docker one.

### Step 6: Update n8n's Webhook URL

n8n needs to know your public URL so it generates correct webhook links. Set this by adding an environment variable to your n8n service in `docker-compose.yml`. Find the `n8n:` service block and look for its `environment:` section. Add this line:

```
- WEBHOOK_URL=https://your-domain.ngrok-free.app
```

Then recreate the n8n container:

```bash
docker compose up -d --force-recreate n8n
```

Now when you open a webhook node in n8n, the Production URL will show your ngrok domain instead of localhost.

---

## Part 4: Updating the Stack

Updating is safe and straightforward. Docker pulls new images and only recreates containers that have updates. Your volumes (where your data lives) are never touched. Check for updates roughly once a month.

### Step 1: Update All Containers at Once

Open the Docker Desktop terminal, navigate to your stack folder, and run:

```bash
cd path/to/self-hosted-ai-starter-kit
docker compose pull
docker compose up -d
```

`docker compose pull` downloads the latest image for every service. `docker compose up -d` recreates any container that now has a newer image. Containers with no update keep running untouched.

> **Tip:** What these commands do exactly
> `pull` only downloads — it doesn't restart anything. `up -d` applies the updates by recreating containers with the new image. Your workflows, credentials, and data stay intact because they live in Docker volumes, not the container itself.

### Step 2: Update a Single Container

To update just one service (for example, n8n only), name it explicitly:

```bash
docker compose pull n8n
docker compose up -d n8n
```

| Container | Update command |
|-----------|----------------|
| n8n | `docker compose pull n8n && docker compose up -d n8n` |
| Postgres | `docker compose pull postgres && docker compose up -d postgres` |
| Ollama | `docker compose pull ollama && docker compose up -d ollama` |
| Qdrant | `docker compose pull qdrant && docker compose up -d qdrant` |
| ngrok | `docker compose pull ngrok && docker compose up -d --force-recreate ngrok` |

### Step 3: Safe Update Checklist

Run through this before and after every update:

- [ ] Your workflows are working before you start (test a webhook or manual run)
- [ ] You know where your `docker-compose.yml` file is (you'll need it if something breaks)
- [ ] Run `docker compose pull` first — just the download, no restarts yet
- [ ] Run `docker compose up -d` to apply the updates
- [ ] Open n8n in your browser and confirm you can log in
- [ ] Check that your active workflows are still published
- [ ] Test a webhook to confirm your ngrok URL still routes correctly
- [ ] Check Docker Desktop — all containers should have green dots

> **Warning:** If something breaks after an update
> Check the logs of the affected container in Docker Desktop. Most issues are either a changed configuration format in a new version, or a container not reconnecting to its volume properly. You can roll back by specifying an older image tag in `docker-compose.yml` — for example, `n8nio/n8n:1.45.0` instead of `n8nio/n8n:latest`.

---

## Common Issues

| Problem | Cause | Fix |
|---------|-------|-----|
| Docker won't start — WSL timeout | WSL 2 process hung or stale session on Windows | Open PowerShell and run `wsl --shutdown`, wait 10 seconds, reopen Docker. If it persists, fully restart your PC. |
| n8n shows "Set up owner account" after recreating the container | New n8n container isn't connecting to the Postgres database — it's using a fresh SQLite instead | Delete the standalone n8n container, then run `docker compose up -d n8n` from inside the starter kit folder to recreate it properly with the right environment variables pointing to Postgres. |
| ngrok container keeps restarting | Invalid or revoked authtoken | Go to the ngrok dashboard, click **Reset Authtoken**, copy the new token, update your `docker-compose.yml`, then run `docker compose up -d --force-recreate ngrok`. |
| ngrok error: ERR_NGROK_8012 / undefined://undefined | ngrok can't reach n8n — they're on different Docker networks | Add `networks: - demo` to the ngrok service in `docker-compose.yml`. The command must use `http://n8n:5678` not `localhost:5678`. |
| ngrok error: ERR_NGROK_334 — endpoint already online | Two ngrok instances running at the same time — one in Docker, one in a terminal | Stop whichever one is running outside Docker (Ctrl+C in the terminal) before starting the Docker ngrok container. |
| Webhook 404 from Meta / Instagram | The workflow isn't published, or the webhook path changed | Make sure the workflow is set to **Published** (not just active in test mode). Check the Production URL in the webhook node matches exactly what's in Meta's callback URL field. |
| Can't delete a Docker image — "image is in use" | A stopped container still references that image | Delete the container first (even if it's stopped), then delete the image. Or just leave it — dangling images don't affect running containers. |

---

*Chelsea Andrea Dominguez · Systems Manual · Docker Stack Guide · v1.0*
