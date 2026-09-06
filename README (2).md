<div align="center">

# 🌐 **Cloudflare Domain Playbook**

### Point **any** domain or subdomain at **anything** — a website, a game server, a media server, a smart-home hub, a database, SSH, or email — through Cloudflare. In plain English, on the free plan.

<br />

[![Status](https://img.shields.io/badge/status-verified-2ea44f?logo=checkmark&logoColor=white&style=for-the-badge&labelColor=1d3557)](https://developers.cloudflare.com/)
[![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?logo=cloudflare&logoColor=white&style=for-the-badge&labelColor=1d3557)](https://cloudflare.com)
[![Free plan](https://img.shields.io/badge/works%20on-FREE%20plan-0077b6?style=for-the-badge&labelColor=1d3557)](https://developers.cloudflare.com/fundamentals/concepts/cloudflare-plans/)
[![Beginner friendly](https://img.shields.io/badge/beginner-friendly-6a4c93?style=for-the-badge&labelColor=1d3557)](https://opensource.org/licenses/MIT)
[![License](https://img.shields.io/badge/license-MIT-grey?style=for-the-badge&labelColor=1d3557)](https://opensource.org/licenses/MIT)

<br />

<sup>✅ Free plan · 🧭 Beginner–friendly · 🔌 Websites · 🎮 Games · 🧰 Self-hosted · 🔒 Remote access · ✉️ Email · ⏱️ ~15 min</sup>

</div>

---

<a name="nav"></a>
<div align="center">

## 🧭 Jump straight to what you need

| 1️⃣ &nbsp;[Choose your path](#choose-your-path) | 2️⃣ &nbsp;[💡 The Big Idea](#the-big-idea) | 3️⃣ &nbsp;[🌐 Websites](#part-2-websites) | 4️⃣ &nbsp;[⚙️ Custom ports](#part-3-custom-ports) | 5️⃣ &nbsp;[🎮 Games](#part-4-games) | 6️⃣ &nbsp;[🧰 Self-hosted](#part-5-selfhosted) | 7️⃣ &nbsp;[🔌 Remote & email](#part-6-remote) | 8️⃣ &nbsp;[🩺 Fix it](#troubleshooting) |
| **Pick a scenario** | **4 types of traffic** | **HTTP apps** | **Non-standard ports** | **TCP/UDP** | **Dashboards & media** | **SSH/RDP/📧** | **Diagnose** |

</div>

<details>
<summary><b>📚 Full Table of Contents</b> <i>(click to expand)</i></summary>

- [⚡ TL;DR — The 4 Golden Rules](#tl-dr)
- [✅ What you'll learn](#learn)
- [🎯 What this covers (and what it doesn't)](#covered)
- [💡 The Big Idea — every app is 1 of 4 traffic types](#the-big-idea)
- [🧭 Choose your path](#choose-your-path)
- [🧰 Prerequisites](#prerequisites)
- [📚 How Cloudflare works (the concepts)](#how-it-works)
  - [🟠 Proxied (orange cloud)](#proxy-orange)
  - [⚪ DNS-only (grey cloud)](#dns-only-grey)
  - [📇 Record types — which one do I use?](#record-types)
- [🌐 Part 1 — Websites & Web Apps (HTTP)](#part-2-websites)
  - [1️⃣ 1.1 — Add your site to Cloudflare](#website-add-site)
  - [2️⃣ 1.2 — Website on a subdomain](#website-subdomain)
  - [3️⃣ 1.3 — Website on the root (no subdomain)](#website-apex)
  - [4️⃣ 1.4 — The proxy toggle (orange vs grey)](#website-proxy)
- [⚙️ Part 2 — Apps on Custom Ports](#part-3-custom-ports)
  - [2.1 — Which ports Cloudflare can proxy](#ports-cloudflare)
  - [2.2 — Option A: Reverse proxy (Nginx / Caddy)](#reverse-proxy)
  - [2.3 — Option B: Cloudflare Tunnel (HTTP)](#tunnel-http)
- [🎮 Part 3 — Game Servers (TCP & UDP)](#part-4-games)
  - [3.1 — The Golden Rule: DNS-only for games / anything not HTTP](#games-dns-only)
  - [3.2 — Minecraft on a subdomain](#minecraft-subdomain)
  - [3.3 — Minecraft on the root domain](#minecraft-apex)
  - [3.4 — Minecraft on a custom port (via SRV)](#minecraft-srv)
  - [3.5 — Minecraft through a Tunnel (TCP)](#minecraft-tunnel)
  - [3.6 — Every other game (Valheim, Rust, Terraria, CS2, …)](#games-list)
- [🧰 Part 4 — Self-Hosted Apps (Dashboards, Media, Cloud)](#part-5-selfhosted)
  - [4.1 — Media servers (Jellyfin, Plex, Emby)](#media-servers)
  - [4.2 — Dashboards, home automation & cloud apps](#dashboards-apps)
  - [4.3 — The easy path for all of them](#apps-easy)
- [🔌 Part 5 — Remote Access (SSH / RDP / VNC)](#part-6-remote)
- [✉️ Part 6 — Email (MX / SPF / DKIM / DMARC)](#part-7-email)
- [🧪 Testing & Verification](#testing)
- [🩺 Troubleshooting](#troubleshooting)
- [❓ FAQ](#faq)
- [🔒 Security & Performance Tips](#security-tips)
- [📖 Glossary](#glossary)
- [🧰 Ready-made templates](#templates)
- [📚 Further reading](#further-reading)
- [📄 License & Contributing](#license)

</details>

---

<a name="tl-dr"></a>
<div align="center">

## ⚡ TL;DR — The 4 Golden Rules

> **Forget the jargon. If you remember only 4 things, you're 90% done.**

| # | The rule | 🔍 In plain English |
|:-:|:---|:---|
| **1** | 🟠 **Browsers = Proxied** (orange) | Anything that loads as `https://…` in a browser (websites, dashboards) → flip the cloud **orange**. You get free SSL, caching & DDoS protection. |
| **2** | ⚪ **Games & apps = DNS-only** (grey) | Anything that connects *directly* (games, SSH, databases) uses raw TCP/UDP, not HTTP → leave the cloud **grey** so traffic flows straight to you. |
| **3** | 🎯 **Custom port?** Use a **reverse proxy** or a **Cloudflare Tunnel** | Cloudflare's proxy only listens on [a fixed list of ports](#ports-cloudflare). If yours isn't in it, Nginx/Caddy or a tunnel fixes it. |
| **4** | 📡 **UDP = no free tunnel** | Cloudflare's **free** Tunnel handles TCP only. UDP stuff (most multiplayer games, voice, Bedrock Minecraft) must be **grey/direct** — or use paid Spectrum. |

</div>

> 🧠 **The one-sentence takeaway:** **Does it load in a browser? → proxy it. Does it connect like a game or tool? → DNS-only.** Everything else is just filling in the right fields.

---

<a name="learn"></a>
## ✅ What you'll learn in this guide

- **The 4 types of traffic** — the single idea that makes *anything* easy to connect.
- **Point a domain at a website** — on a subdomain *and* on the bare root.
- **Expose an app on a custom port** — via a reverse proxy or Cloudflare Tunnel.
- **Connect a domain to a game server** — Minecraft (Java & Bedrock) plus Valheim, Rust, Terraria, CS2 and more.
- **Expose self-hosted apps** — Jellyfin, Home Assistant, Nextcloud, Grafana, and friends.
- **Securely reach your computer remotely** — SSH, RDP, VNC — without opening ports.
- **Set up email on your domain** — MX, SPF, DKIM, DMARC.
- **Test, debug, and harden** the whole thing.

---

<a name="covered"></a>
## 🎯 What this covers (and what it doesn't)

| ✅ Covered here | ❌ Not covered here |
|:---|:---|
| Free-plan **DNS, proxy, Tunnel, SRV & email records** | Paid **Spectrum** (for arbitrary UDP / advanced game traffic) |
| Making *anything* on your server **reachable** at a clean URL | Building the app/server software itself (that's your stack) |
| **Cloudflare-side** configuration | Deep OS/firewall hardening (I point you in the right direction) |

> 💡 This guide is about **connecting the domain** — making your server's stuff reachable *at* a clean web address. It assumes your website, game, or app is already running and listening on its port.

---

<a name="the-big-idea"></a>
## 💡 The Big Idea — every app is 1 of 4 traffic types

> **This is the unlock.** Cloudflare doesn't care *what* your app is. It only cares about the **type of traffic** it speaks. Once you classify it, the setup is the same for everything.

### 🔍 How to classify anything in 3 seconds

> **Ask one question:** *"Does it open in a browser as `http://` or `https://`?"*

| Answer | It's a… | What you do |
|:---|:---:|:---|
| ✅ Yes, it's a web page/dashboard | 🌐 **"Web"** | **Proxy** it (orange) 🟠 — or Tunnel |
| ❌ It's a game/tool that connects directly | 🔌 **"TCP"** or 📡 **"UDP"** | **DNS-only** (grey) ⚪ — or Tunnel for TCP |
| ✉️ It's email for your domain | ✉️ **"Email"** | **DNS-only** MX + SPF/DKIM/DMARC |

### The master table

| Type | Speaks | Free proxy? | How to expose it | Examples |
|:---|:---|:---:|:---|:---|
| 🌐 **Web** | HTTP / HTTPS | ✅ Yes | Proxy it 🟠, or Tunnel with `http://` | Website, Jellyfin, Home Assistant, Nextcloud, Grafana |
| 🔌 **TCP** | TCP | ⚪ DNS-only, or Tunnel `tcp://` | Grey it ⚪, or Tunnel | Minecraft **Java**, SSH, RDP, databases |
| 📡 **UDP** | UDP | ⚪ Direct only | Grey it ⚪ (direct) or paid Spectrum for the tunnel | Minecraft **Bedrock**, most multiplayer games, VoIP |
| ✉️ **Email** | SMTP / IMAP / POP | ❌ Never | DNS-only MX + SPF/DKIM/DMARC | Sending & receiving mail on your domain |

> 🔎 **Notice the pattern:** ☁️ **browsers (Web) are proxied**, 🔌 **TCP is either grey or tunneled**, 📡 **UDP is always grey** (free tunnel doesn't do UDP), ✉️ **email is never proxied.** Once you know the type, you just follow that recipe.

---

<a name="choose-your-path"></a>
<div align="center">

## 🧭 Choose your path — find your situation

</div>

| 🎯 You want to… | 🔧 The fix | 📂 Jump to |
|:---|:---|:---|
| Host a **website** (normal port 80/443) | Proxied `A` record | [Part 1](#part-2-websites) |
| Website on a **subdomain** (`app.yoursite.com`) | Proxied `A` (+ optional `CNAME`) | [1.2](#website-subdomain) |
| Website on the **root** (`yoursite.com`) | Proxied `A` with name `@` | [1.3](#website-apex) |
| **Web app on a custom port** (e.g. `3000`) + public IP | Reverse proxy on 80/443 → app | [2.2](#reverse-proxy) |
| **Web app on a custom port** + no public IP / hide IP | Cloudflare Tunnel (HTTP) | [2.3](#tunnel-http) |
| **Minecraft** (`play.yoursite.com`) | DNS-only `A` (+ `SRV` for custom port) | [3.2](#minecraft-subdomain) |
| **Minecraft** on the root (`yoursite.com`) | DNS-only root + `SRV` | [3.3](#minecraft-apex) |
| **Minecraft on a custom port** (Java) | `SRV` record | [3.4](#minecraft-srv) |
| **Minecraft** hiding its IP / behind NAT | Cloudflare Tunnel (TCP) | [3.5](#minecraft-tunnel) |
| **Valheim / Rust / Terraria / CS2 / other game** | DNS-only direct (or paid Spectrum) | [3.6](#games-list) |
| **Jellyfin / Plex / Home Assistant / Nextcloud…** | Proxy or Tunnel (HTTP) | [Part 4](#part-5-selfhosted) |
| **SSH / RDP / VNC** from anywhere, securely | Cloudflare Tunnel (TCP / SSH) | [Part 5](#part-6-remote) |
| **Email** on your domain | MX + SPF / DKIM / DMARC | [Part 6](#part-7-email) |
| Not sure? | Read the [Big Idea](#the-big-idea) first | ⬅️ |

---

<a name="prerequisites"></a>
## 🧰 Prerequisites

| ✅ | What you need | 👀 Why |
|:-:|:---|:---|
| 🏷️ | **A domain** you control (e.g. `example.com`) | You'll point its DNS at Cloudflare. |
| ☁️ | **A Cloudflare account** — [free to sign up](https://dash.cloudflare.com/sign-up) | Everything here works on the **Free** plan. |
| 📍 | **Your server's public IP** | The machine hosting your stuff must be reachable. (No public IP / behind NAT? Use the [Tunnel](#tunnel-http) / [game Tunnel](#minecraft-tunnel) options.) |
| 🔌 | **Port forwarding** *(only for direct connections from home)* | Forward the needed ports on your router to the server's LAN IP. |
| 🐳 *(optional)* | **Docker** | The easiest way to run most self-hosted apps. |

---

<a name="how-it-works"></a>
## 📚 How Cloudflare works — the concepts

> Read this once (2 minutes) and every section below becomes obvious. Cloudflare sits **in front of** your server and decides how to handle each request.

```
  ┌──────────────────────────────────────────────────────────────────────┐
  │                            THE INTERNET                              │
  │   Visitors / players  ────────────────────────────────────┐          │
  │                                                          ▼          │
  │                            ┌────────────────────────────────┐      │
  │                            │       CLOUDFLARE (edge)        │      │
  │                            │  DNS  +  Proxy  +  SSL  + Cache │      │
  │                            │  +  DDoS protection            │      │
  │                            └───────────────┬────────────────┘      │
  │                                            │                        │
  │                    ┌───────────────────────▼────────────────┐      │
  │                    │              YOUR SERVER                │      │
  │                    │   • Web app   (port 3000 / 8080 / …)    │      │
  │                    │   • Game      (TCP/UDP port 25565 / …)  │      │
  │                    │   • Reverse proxy  (port 80 / 443)      │      │
  │                    │   • SSH / RDP  (port 22 / 3389 / …)     │      │
  │                    └─────────────────────────────────────────┘      │
  └──────────────────────────────────────────────────────────────────────┘
```

> 🧠 **The core idea:** Cloudflare's **proxy** only speaks **HTTP / HTTPS**. So: **browser stuff → proxy (orange)**, **game/tool stuff → grey**, and **not-sure → read the Big Idea again.**

<a name="proxy-orange"></a>
### 🟠 Proxied (orange cloud)

| | |
|:---|:---|
| **What it does** | Cloudflare stands in front and **handles the connection**. It decrypts TLS, caches assets, blocks attacks, and **hides your real IP**. |
| **What it's for** | Anything over **HTTP / HTTPS** — websites, web apps, dashboards, media UIs. |
| **Look for** | The **orange** cloud icon 🟠 next to a DNS record. |

> ✅ **Your site goes *through* Cloudflare.** Free SSL cert, CDN caching, DDoS protection. Your visitors see Cloudflare's IP, not yours.

<a name="dns-only-grey"></a>
### ⚪ DNS-only (grey cloud)

| | |
|:---|:---|
| **What it does** | Cloudflare **only answers DNS lookups**. Traffic then goes **straight** to your server's IP — Cloudflare does not touch it. |
| **What it's for** | Anything **that isn't HTTP**: games (TCP/UDP), SSH, RDP, databases, email (MX). |
| **Look for** | The **grey** cloud icon ⚪ next to a DNS record. |

> ⚠️ **This is the #1 mistake.** People leave a game record **proxied** (orange) and it silently fails. **Games and direct-connect apps must be grey.**

<a name="record-types"></a>
### 📇 Record types — which one do I use?

| Record | What it does | Used for | Proxiable? |
|:---|:---|:---|:---:|
| **A** | Name → **IPv4 address** | Websites, games — everything | 🟠 (HTTP) / ⚪ (TCP) |
| **AAAA** | Name → **IPv6 address** | IPv6-only servers | 🟠 / ⚪ |
| **CNAME** | Name → **another name** | Subdomains (`www` → `app`) | 🟠 |
| **SRV** | "Use **this host on this port**" | Minecraft custom port, other services | ⚪ (never) |
| **MX** | Email routing | Email | ⚪ (never) |
| **TXT / CAA** | Verification, security | SPF, DKIM, DMARC, cert authority | ⚪ |

---

<a name="part-2-websites"></a>
# 🌐 Part 1 — Websites & Web Apps (HTTP)

> **Goal:** `example.com` (root) and/or `app.example.com` (subdomain) loads your site **through** Cloudflare — with **free SSL, caching, and DDoS protection**.

> 💡 This covers **any** browser-based app: a website, a dashboard, a media UI — anything that's really just an HTTP server.

<a name="website-add-site"></a>
## 1️⃣ 1.1 — Add your site to Cloudflare

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com).
2. Click **+ Add a domain** → **Add a site**.
3. Enter your domain, choose the **Free** plan → **Continue**.
4. Cloudflare gives you **two nameservers** (e.g. `alice.ns.cloudflare.com`, `bob.ns.cloudflare.com`).
5. At your **domain registrar**, find **Nameservers / DNS** and replace the current ones with **Cloudflare's two**.
6. Wait for the zone to show **Active**. 🕐 Propagation is usually minutes but can be up to 24h.

> ⚠️ **Email warning:** If you get email on this domain, keep the **MX** records **DNS-only** (grey). See [Part 6](#part-7-email). Email is never proxied.

<a name="website-subdomain"></a>
## 2️⃣ 1.2 — Website on a subdomain

Want `app.example.com`? Add one `A` record.

**DNS → Records → Add record →** fill in:

| Field | Value | Notes |
|:---|---|:---|
| **Type** | `A` | — |
| **Name** | `app` | becomes `app.example.com` |
| **IPv4 address** | `203.0.113.50` | your server's public IP (no port!) |
| **Proxy status** | 🟠 **Proxied** | it's a web page, keep it orange |
| **TTL** | `Auto` | — |

> ✅ **Done.** `https://app.example.com` loads through Cloudflare. The SSL cert (Universal SSL) issues automatically within ~15 minutes.

**Want `www.example.com` to hit the same site?** Add a `CNAME`:

| Type | Name | Target | Proxy status | TTL |
|:---|:---|:---|:---:|:---:|
| `CNAME` | `www` | `app.example.com` | 🟠 Proxied | Auto |

<a name="website-apex"></a>
## 3️⃣ 1.3 — Website on the root (no subdomain)

Want the bare domain `example.com`? Use `@` (the root).

| Field | Value | Notes |
|:---|---|:---|
| **Type** | `A` | — |
| **Name** | `@` | the root / apex |
| **IPv4 address** | `203.0.113.50` | your server's public IP |
| **Proxy status** | 🟠 **Proxied** | — |
| **TTL** | `Auto` | — |

> 🔁 **Make both `www` and the root work** (optional): Cloudflare **Rules → Redirect Rule** can bounce `www` ↔ root, so only one origin serves the site.

<a name="website-proxy"></a>
## 4️⃣ 1.4 — The proxy toggle (orange vs grey)

The cloud icon next to each record is the single most important control:

| Icon | Status | Use for | Cloudflare does |
|:---:|:---|:---|:---|
| 🟠 | **Proxied** | Websites, web apps, dashboards | TLS, caching, DDoS protection, hides IP |
| ⚪ | **DNS only** | Games, SSH, RDP, email | Only answers DNS; traffic goes straight to you |

> 🔧 **Click the icon to toggle it per record.** It's **per-record**, not per-domain — so proxy your website and keep your game grey at the same time.

**After saving, wait ~15 min**, then open your domain over `https://`. ✅

---

<a name="part-3-custom-ports"></a>
# ⚙️ Part 2 — Apps on Custom Ports

> **Goal:** Your app runs on a non-standard port (e.g. `3000`, `8080`, `9000`) but users reach it at the clean `https://example.com`.

<a name="ports-cloudflare"></a>
## 2.1 — Which ports Cloudflare can proxy

Cloudflare's free proxy listens on **exactly** these ports:

| 🟠 HTTP ports | 🟠 HTTPS ports |
|:---|:---|
| `80`, `8080`, `8880`, `2052`, `2082`, `2086`, `2095` | `443`, `2053`, `2083`, `2087`, `2096`, `8443` |

> ⚠️ **The catch:** If your app is on a port **not** in that list (like `3000` or `25565`), Cloudflare **won't proxy it directly**. You have two clean fixes below.

| 🛠️ | Method | When to use | Pros | Cons |
|:---:|:---|:---|:---|:---|
| **A** | [Reverse proxy](#reverse-proxy) | You have a **public IP** | Production-grade, flexible | Needs Nginx/Caddy setup |
| **B** | [Cloudflare Tunnel](#tunnel-http) | **No public IP** / behind NAT / hide IP | No port forwarding, hides origin, works for any port | Adds a layer |

<a name="reverse-proxy"></a>
## 2.2 — Option A: Reverse proxy (Nginx / Caddy)

**The standard way.** You run a reverse proxy on ports **80** and **443** that forwards inbound traffic to your app's real port. Cloudflare then proxies 80/443 normally.

**1.** Add an **`A` record** for `@` (or a subdomain) → your server's IP, **proxied** 🟠.

**2.** Install & configure **Nginx** to forward to your app:

```nginx
# /etc/nginx/sites-available/example.com
server {
    listen 80;
    server_name example.com www.example.com;

    location / {
        proxy_pass         http://127.0.0.1:3000;   # <-- YOUR APP PORT
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade           $http_upgrade;   # WebSockets
        proxy_set_header   Connection        "upgrade";
    }
}
```

Enable it and reload:

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

**💡 Prefer Caddy?** It auto-generates HTTPS and proxies in **two lines**:

```caddyfile
# Caddyfile
example.com, www.example.com {
    reverse_proxy 127.0.0.1:3000     # <-- YOUR APP PORT
}
```

**3.** Point the proxy at `http://127.0.0.1:3000` and let Cloudflare handle TLS. **Done.**

> ✅ **Result:** `https://example.com` → Cloudflare (443) → Nginx (443) → your app (`3000`). No custom port exposed; everything proxied.

<a name="tunnel-http"></a>
## 2.3 — Option B: Cloudflare Tunnel (HTTP)

**No public IP, no port forwarding, and your origin IP stays hidden.** Cloudflare Tunnel (`cloudflared`) opens an outbound encrypted connection to Cloudflare and reaches **any** local port.

**1. Install `cloudflared`** on the server:

```bash
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -o cloudflared
chmod +x cloudflared
sudo mv cloudflared /usr/local/bin/
```

**2. Log in and create a named tunnel:**

```bash
cloudflared tunnel login
cloudflared tunnel create my-app-tunnel
```

**3. Point your hostname at the app.** Create `~/.cloudflared/config.yml`:

```yaml
tunnel: <YOUR-TUNNEL-ID>
credentials-file: /home/user/.cloudflared/<YOUR-TUNNEL-ID>.json

ingress:
  - hostname: app.example.com
    service: http://localhost:3000        # <-- YOUR APP PORT
  - hostname: example.com
    service: http://localhost:3000
  - service: http_status:404              # catch-all
```

**4. Route DNS to the tunnel, then run it:**

```bash
cloudflared tunnel route dns my-app-tunnel app.example.com
cloudflared tunnel run my-app-tunnel
```

**5. Make it a service so it survives reboots:**

```bash
sudo cloudflared service install
sudo systemctl enable --now cloudflared
```

> ✅ **Result:** `https://app.example.com` → Cloudflare → encrypted tunnel → your app on `localhost:3000`. Real IP never public, **any** port works.

---

<a name="part-4-games"></a>
# 🎮 Part 3 — Game Servers (TCP & UDP)

> **Goal:** Players type a clean domain (no ugly `IP:port`) to join your server.

<a name="games-dns-only"></a>
## 3.1 — The Golden Rule: DNS-only for games

> 🚨 **Games speak raw TCP and UDP, NOT HTTP.** Cloudflare's orange proxy only handles HTTP/HTTPS. **Every game-related `A` record must be GREY (DNS-only).** A proxied record *will* break the connection.

And the other half of the story:

| Edition / game | Protocol | Reads SRV? | Free Tunnel? |
|:---|:---|:---:|:---:|
| **Minecraft (Java)** | TCP | ✅ | ✅ TCP + SRV |
| **Minecraft (Bedrock)** | UDP | ❌ | ❌ (direct only) |
| Most other games | UDP | ❌ | ❌ (UDP needs paid Spectrum) |

> 🔎 **Why Minecraft Java is special:** it's the rare game that's **TCP** *and* supports **SRV records**. That's why you can have a clean `play.example.com`. Almost every other game uses **UDP**, which the free tunnel can't carry — so for those you point DNS at your real IP (grey) or pay for Spectrum.

<a name="minecraft-subdomain"></a>
## 3.2 — Minecraft on a subdomain

Use `play.example.com`. (Recommended — keeps your website proxy untouched.)

**1.** Create an `A` record, **DNS-only**:

| Field | Value | Notes |
|:---|---|:---|
| **Type** | `A` | — |
| **Name** | `play` | becomes `play.example.com` |
| **IPv4 address** | `203.0.113.50` | your server's public IP |
| **Proxy status** | ⚪ **DNS only** | **must be grey** |
| **TTL** | `Auto` | — |

**2. On the default port `25565`?** That's it — players join `play.example.com`. 🎉

<a name="minecraft-apex"></a>
## 3.3 — Minecraft on the root domain

Want players to join just `example.com`? Add an **SRV record on the root**.

**1.** Create the root `A` record, **DNS-only**:

| Type | Name | Content | Proxy status | TTL |
|:---|:---|:---|:---:|:---:|
| `A` | `@` | `203.0.113.50` | ⚪ **DNS only** | Auto |

**2.** Add the root **SRV record**:

| Type | Name | Priority | Weight | Port | Target | TTL |
|:---|:---|:---:|:---:|:---:|:---|:---:|
| `SRV` | `_minecraft._tcp` | `0` | `5` | `25565` | `example.com` | Auto |

> The Name `_minecraft._tcp` resolves to `_minecraft._tcp.example.com`. Players then join `example.com`.

> ⚠️ **Root conflict:** If the same `@` record also runs a website, they can't both use the root cleanly. **Either** move the website to `www` and let `@` be DNS-only Minecraft, **or** use a `play.` subdomain. [See FAQ](#faq).

<a name="minecraft-srv"></a>
## 3.4 — Minecraft on a custom port (via SRV)

Same as above, but change the **port**. An SRV is the only way to let Java players type a clean name *and* use a non-25565 port.

```text
_minecraft._tcp.play.example.com  SRV  0  5  49001  play.example.com
```

| Field | Value |
|:---|:---|
| **Priority** | `0` |
| **Weight** | `5` |
| **Port** | `49001` *(your server's real port)* |
| **Target** | `play.example.com` *(DNS-only subdomain)* |

> ⚠️ **Two gotchas:** (1) In Cloudflare's UI put `_minecraft._tcp.play` in the **Name** box (Cloudflare renamed the field). (2) The **Target** must be a hostname (an existing `A` record), **not** an IP, and it must stay **DNS-only** too.

> 🔁 **Double-check:** Java reads SRV automatically. **Bedrock does NOT** — Bedrock players must type `play.example.com:49001` (port separately).

<a name="minecraft-tunnel"></a>
## 3.5 — Minecraft through a Tunnel (TCP)

> **Best if you have no public IP, are behind CGNAT/NAT, or want to hide your server's IP.** Uses Cloudflare Tunnel's **arbitrary TCP** — works on the **free plan**.

**1.** Add the Minecraft hostname as a `tcp` service in `~/.cloudflared/config.yml`:

```yaml
tunnel: <YOUR-TUNNEL-ID>
credentials-file: /home/user/.cloudflared/<YOUR-TUNNEL-ID>.json

ingress:
  - hostname: play.example.com
    service: tcp://127.0.0.1:25565      # <-- your Minecraft port
  - service: http_status:404
```

**2.** Route DNS & run the tunnel:

```bash
cloudflared tunnel route dns mc-tunnel play.example.com
cloudflared tunnel run mc-tunnel
```

**3.** Each player/client opens a **local bridge**, then joins `localhost`:

```bash
cloudflared access tcp --hostname play.example.com --url localhost:25565
```

…then in Minecraft, join **`localhost:25565`**.

> ⚠️ **Honest limitation:** The free tier tunnels **TCP** only — great for **Minecraft Java**. **Bedrock uses UDP**, which the free tier can't tunnel; you'd need **Spectrum (Enterprise)** or a UDP alternative. Set SSL mode to **"Full (strict)"** if `cloudflared` reports cert errors.

<a name="games-list"></a>
## 3.6 — Every other game (Valheim, Rust, Terraria, CS2, …)

Here's the thing: **most modern multiplayer games use UDP.** The free Cloudflare Tunnel does **TCP only**, so for UDP games you can't use the free tunnel. Your options:

| 🛠️ | Option | Works for | Note |
|:---:|:---|:---|:---|
| **1** | **DNS-only (grey) + real IP** (`play.example.com` → IP) | TCP & UDP games | Players connect to `play.example.com:PORT`. Simplest & free. |
| **2** | **Cloudflare Tunnel (TCP)** | TCP games only | Hides IP, free. Not for UDP games. |
| **3** | **Cloudflare Spectrum** | TCP & UDP | Paid (Enterprise). Only if you need the proxy's IP-hiding for UDP. |

**The quick reference (default ports — always check the game's own docs, they vary):**

<details>
<summary><b>📋 Click to expand: common game ports & protocol</b></summary>

| Game | Protocol | Default port(s) | Free Tunnel? |
|:---|:---:|:---:|:---:|
| Minecraft (Java) | TCP | 25565 | ✅ |
| Minecraft (Bedrock) | UDP | 19132 | ❌ |
| Valheim | UDP | 2456, 2457–2458 | ❌ |
| Terraria | TCP | 7777 | ✅ |
| Rust | UDP | 28015 (+28082 RCON) | ❌ |
| Counter-Strike 2 | UDP | 27015 | ❌ |
| GTA 5 (FiveM) | TCP+UDP | 30120 | ⚠️ partial |
| ARK | UDP | 7777, 7778 | ❌ |
| TeamSpeak 3 | UDP | 9987 (voice) | ❌ |
| Palworld | UDP | 8211 | ❌ |
| Garry's Mod | UDP | 27015 | ❌ |
| 7 Days to Die | UDP | 26900 (+web 8080) | ❌ |
| Don't Starve Together | UDP | 10999 | ❌ |
| Project Zomboid | UDP | 16261 | ❌ |
| Satisfactory | UDP | 7777 (+15000) | ❌ |

</details>

> 🧭 **Bottom line for non-Minecraft games:** set the subdomain to **DNS-only (grey)**, add an `A` record pointing at your IP, and have players connect to `play.example.com:PORT`. If you have no public IP or *must* hide it, you'll likely need a UDP tunneling service or Spectrum.

---

<a name="part-5-selfhosted"></a>
# 🧰 Part 4 — Self-Hosted Apps (Dashboards, Media, Cloud)

> **Goal:** Reach your self-hosted apps at a clean `https://…` URL instead of `http://localhost:PORT` or an ugly IP:port.

> 💡 **Good news:** almost every self-hosted app is a **Web** app (HTTP). So you can **proxy it** if it's on a [proxied port](#ports-cloudflare), or more commonly, **tunnel it**.

**The universal recipe for any self-hosted app:**

| Method | When | Steps |
|:---|:---|:---|
| 🟠 **Proxy** | App on ports 80/443 or a [listed port](#ports-cloudflare) | Add proxied `A` record |
| 🌐 **Tunnel (recommended)** | App on *any* port, no public IP, hide IP | Add tunnel with `service: http://localhost:PORT` |

<a name="media-servers"></a>
## 4.1 — Media servers (Jellyfin, Plex, Emby)

These stream video/audio. **All are Web apps** — proxy or tunnel them.

| App | What it does | Default port | How |
|:---|:---|:---:|:---|
| **Jellyfin** | Open-source media server | 8096 | 🟠 proxy **or** 🌐 tunnel |
| **Plex** | Media server + streaming | 32400 | 🌐 tunnel |
| **Emby** | Media server | 8096 | 🌐 tunnel |
| **Navidrome** | Music streaming | 4533 | 🌐 tunnel |

> 🎬 **Tunnel them.** Media streaming can be heavy, but Cloudflare Tunnel handles it fine for home use. For best quality, keep your origin IP exposed via DNS-only and skip the proxy. ⚠️ Cloudflare's ToS restricts *some* large streaming/video uses — check for your use case.

<a name="dashboards-apps"></a>
## 4.2 — Dashboards, home automation & cloud apps

All **Web** apps — the same recipe.

| App | What it does | Default port | How |
|:---|:---|:---:|:---|
| **Home Assistant** | Smart-home hub | 8123 | 🟠 / 🌐 |
| **Nextcloud** | Private cloud storage | 8080 (web) | 🟠 / 🌐 |
| **Grafana** | Dashboards | 3000 | 🟠 / 🌐 |
| **Uptime Kuma** | Uptime monitoring | 3001 | 🟠 / 🌐 |
| **Portainer** | Docker dashboard | 9000 | 🟠 / 🌐 |
| **Vaultwarden** | Password manager | 8000 | 🟠 / 🌐 |
| **Gitea / Forgejo** | Self-hosted Git | 3000 | 🟠 / 🌐 |
| **n8n** | Automation workflows | 5678 | 🟠 / 🌐 |
| **Pi-hole (web UI)** | Ad blocker | 80 (web) | 🟠 / 🌐 |
| **Synology DSM** | NAS dashboard | 5000/5001 | 🌐 |

<a name="apps-easy"></a>
## 4.3 — The easy path for all of them

**Option A — Proxy** (app on a proxied port):
1. Add `A` record `@` or subdomain → your IP, **proxied** 🟠.
2. If the app is on a non-listed port, wrap it with a [reverse proxy](#reverse-proxy).

**Option B — Tunnel** (works for any port & hides your IP) — the most beginner-friendly:
1. Add to `~/.cloudflared/config.yml`:
   ```yaml
   ingress:
     - hostname: home.example.com
       service: http://localhost:8123      # <-- app's port
     - service: http_status:404
   ```
2. `cloudflared tunnel route dns my-tunnel home.example.com`
3. `cloudflared tunnel run my-tunnel`

> ✅ Now `https://home.example.com` → Cloudflare → encrypted tunnel → your app. **No ports opened, no public IP needed.**

> 🔐 **Security note:** For anything with logins (Home Assistant, Nextcloud, Vaultwarden), enable Cloudflare **Access** to require a login *before* reaching the app, and use HTTP auth / strong passwords. More in [Security](#security-tips).

---

<a name="part-6-remote"></a>
# 🔌 Part 5 — Remote Access (SSH / RDP / VNC)

> **Goal:** Reach your computer's SSH, RDP, or VNC securely from anywhere — without opening a port on your router.

**These are TCP** (or UDP for some VNC). Use **Cloudflare Tunnel + `cloudflared access`** on both sides. This is far safer than exposing ports because the connection is encrypted and you can require a login.

**1.** Add the service to `~/.cloudflared/config.yml`:

```yaml
ingress:
  # SSH
  - hostname: ssh.example.com
    service: ssh://127.0.0.1:22
  # RDP (Windows Remote Desktop)
  - hostname: rdp.example.com
    service: tcp://127.0.0.1:3389
  # VNC
  - hostname: vnc.example.com
    service: tcp://127.0.0.1:5900
  - service: http_status:404
```

**2.** Route DNS & run the tunnel (on the machine you want to reach):

```bash
cloudflared tunnel route dns remote-tunnel ssh.example.com
cloudflared tunnel run remote-tunnel
```

**3.** Connect from the **client** machine:

**SSH** — Cloudflare has a built-in helper:

```bash
cloudflared access ssh --hostname ssh.example.com
# then add to ~/.ssh/config:
#   Host myserver
#       ProxyCommand cloudflared access ssh --hostname ssh.example.com
```

**RDP / VNC** — open a local bridge then connect to `localhost`:

```bash
cloudflared access tcp --hostname rdp.example.com --url localhost:3389
cloudflared access tcp --hostname vnc.example.com --url localhost:5900
```

> ✅ Then point your RDP/VNC client at `localhost:3389` / `localhost:5900`. The traffic travels through Cloudflare, encrypted, and (with Access) requires your login. **No public ports, no exposed IP.**

> 💡 **Tip:** Turn on a Cloudflare **Access** policy to require a login before the connection is allowed. That way even with the right hostname, only *you* can connect.

---

<a name="part-7-email"></a>
# ✉️ Part 6 — Email (MX / SPF / DKIM / DMARC)

> **Goal:** Your domain can send *and* receive email — without breaking deliverability.

**⚠️ Critical:** Email records are **NEVER proxied.** They must be **DNS-only (grey).** This is separate from your website.

**1.** Add your email provider's **MX record** (DNS-only):

| Type | Name | Priority | Content | Proxy status |
|:---|:---|:---:|:---|:---:|
| `MX` | `@` | `10` | `mail.yourprovider.com` | ⚪ DNS only |

**2.** Add **SPF** (who is allowed to send as you) — a `TXT` record:

| Type | Name | Content |
|:---|:---|:---|
| `TXT` | `@` | `v=spf1 include:_spf.yourprovider.com ~all` |

**3.** Add **DKIM** (signs your outgoing mail) — another `TXT` record, given to you by your provider (looks like `mail._domainkey` with a long key).

**4.** Add **DMARC** (tells receivers what to do with bad mail):

| Type | Name | Content |
|:---|:---|:---|
| `TXT` | `_dmarc` | `v=DMARC1; p=quarantine; rua=mailto:you@example.com` |

> ✅ **The takeaway:** You (or your email host) add these 4 record types, all **DNS-only**. Your website can still be proxied separately — email never goes through the proxy.

---

<a name="testing"></a>
# 🧪 Testing & Verification

Confirm your records resolve before telling anyone to connect.

**1. Check your `A`/`AAAA` record:**

```bash
nslookup app.example.com
# or
dig app.example.com
```

**2. Check an `SRV` record (Minecraft, etc.):**

```bash
nslookup -type=SRV _minecraft._tcp.play.example.com
# or
dig SRV _minecraft._tcp.play.example.com
```

**3. Check email records:**

```bash
dig MX example.com
dig TXT example.com        # SPF
dig TXT _dmarc.example.com
```

**4. Test the website from another network** (e.g. a phone on mobile data) — rules out local DNS caching.

**5. In the game / app client**, connect using your clean domain.

> ✅ **Success looks like:** DNS returns your IP/port, the site loads over `https://`, and the client actually connects.

---

<a name="troubleshooting"></a>
# 🩺 Troubleshooting

| 😟 Symptom | 🧐 Likely cause | ✅ Fix |
|:---|:---|:---|
| Website won't load / SSL error | Cert not issued yet, or not proxied | Wait ~15 min; confirm proxy is 🟠; set SSL to **Full (strict)**. |
| Game "connection timed out" | Record is **proxied** 🟠 | Click the cloud → **DNS only** ⚪. |
| Game "can't reach server" | Wrong port / missing SRV | Verify the SRV record; target must be a hostname, port must match. |
| SRV record not working | Proxy on, or target is an IP | Target must be a **DNS-only hostname**, not an IP. |
| Only Bedrock fails | Bedrock doesn't read SRV | Join with `play.example.com:PORT` explicitly. |
| Root (`@`) conflict (site + game) | Both trying to use the root | Move site to `www`, or use a subdomain for the game. |
| Works at home, fails elsewhere | Local DNS cache | Test from another network, or flush DNS / use `1.1.1.1`. |
| Email breaks after switching to Cloudflare | MX proxied / gone | Keep **MX** records **DNS-only** ⚪ and re-add if Missing. |
| Custom port won't proxy | Port not in the [proxy list](#ports-cloudflare) | Use a [reverse proxy](#reverse-proxy) or [Cloudflare Tunnel](#tunnel-http). |
| UDP game won't tunnel | Free Tunnel doesn't do UDP | Use **DNS-only** direct, or paid **Spectrum**. |

---

<a name="faq"></a>
# ❓ FAQ

<details>
<summary><b>Q: Can I proxy my website AND run a game on the same domain?</b></summary>
<p>Yes — as long as they're on <strong>different records</strong>. Keep the website record <strong>proxied</strong> 🟠 and the game record <strong>DNS-only</strong> ⚪. The proxy toggle is per-record, not per-domain.</p>
</details>

<details>
<summary><b>Q: How do I know if something is "Web", "TCP", or "UDP"?</b></summary>
<p>If it opens in a browser as <code>http(s)://</code>, it's <strong>Web</strong> → proxy it. If it's a game or tool that connects directly (not a browser), check whether it uses <strong>TCP</strong> or <strong>UDP</strong>. See <a href="#the-big-idea">The Big Idea</a>.</p>
</details>

<details>
<summary><b>Q: Does Cloudflare proxy game traffic?</b></summary>
<p>Not by default — games are raw TCP/UDP. Use a <strong>DNS-only</strong> record, or hide your IP with a <strong>Cloudflare Tunnel</strong> (TCP only on the free plan). UDP games need a direct grey record or paid Spectrum.</p>
</details>

<details>
<summary><b>Q: Is all of this free?</b></summary>
<p>Yes — DNS, proxy, Universal SSL, and Cloudflare Tunnel are all on the <strong>Free</strong> plan. Only <strong>Spectrum</strong> (for arbitrary UDP / advanced game traffic) is a paid Enterprise feature.</p>
</details>

<details>
<summary><b>Q: My custom port isn't in the proxy list. Now what?</b></summary>
<p>Two options: run a <strong>reverse proxy</strong> (Nginx/Caddy) on 80/443 to forward to your app, or use a <strong>Cloudflare Tunnel</strong> which can reach any local port. Both are in <a href="#part-3-custom-ports">Part 2</a>.</p>
</details>

<details>
<summary><b>Q: How long does DNS propagation take?</b></summary>
<p>A few minutes up to 24 hours (usually < 1 hour for Cloudflare's nameservers). Cloudflare's own records are live quickly; external resolvers may lag.</p>
</details>

<details>
<summary><b>Q: Can I reach my home computer (SSH/RDP) without opening a port?</b></summary>
<p>Yes — use a <strong>Cloudflare Tunnel</strong> with <code>cloudflared access</code> (see <a href="#part-6-remote">Part 5</a>). No port forwarding needed, and your IP stays hidden.</p>
</details>

<details>
<summary><b>Q: Why won't my UDP game tunnel through Cloudflare?</b></summary>
<p>Cloudflare's <strong>free</strong> Tunnel only carries TCP. UDP (most multiplayer games, voice chat) requires a direct <strong>DNS-only</strong> record, or the paid <strong>Spectrum</strong> product. This is a Cloudflare limitation, not a setup error.</p>
</details>

---

<a name="security-tips"></a>
# 🔒 Security & Performance Tips

- 🌐 **Use "Full (strict)" SSL** when your origin has its own valid cert.
- 🛡️ **Turn on "Under Attack Mode"** (dashboard) when you're under a DDoS flood.
- 🔐 **Enable Cloudflare Access** to require a login before reaching private apps (Home Assistant, Nextcloud, SSH, RDP).
- 🧱 **Add a WAF / IP allowlist rule** (*Security → WAF*) to lock down admin panels.
- 🚫 **Block non-80/443 ports** with the Pro WAF rule if you expose extra HTTP ports.
- 🔑 **Use API tokens** instead of your global API key when scripting.
- ⏱️ **Enable caching** (*Caching → Cache Rules*) and **Auto Minify** for static sites.
- 🔒 **Never paste your origin IP** openly — use a tunnel or hide it where possible.
- ⚠️ **Keep your domain's DNS point-of-contact current** so you don't lose access.

---

<a name="glossary"></a>
# 📖 Glossary

> Quick reference. Come back if you forget a term.

| Term | Meaning |
|:---|:---|
| **Domain / Zone** | The name you own, e.g. `example.com`. |
| **Subdomain** | A prefix, e.g. `play.example.com`. |
| **Apex / Root** | The bare domain, `example.com` (written as `@` in DNS). |
| **DNS record** | A line in DNS that maps a name to an IP, host, port, etc. |
| **Proxy (orange) 🟠** | Cloudflare handles your HTTP traffic. |
| **DNS-only (grey) ⚪** | Cloudflare only answers DNS; traffic goes straight to your server. |
| **Origin server** | Your actual server (behind Cloudflare). |
| **Cloudflare Tunnel** | An encrypted bridge (`cloudflared`) from your server out to Cloudflare. |
| **SRV record** | Tells a client the *host and port* for a service (how Minecraft finds custom ports). |
| **TCP** | A reliable, ordered data connection (Minecraft Java, SSH). |
| **UDP** | A fast, connectionless data stream (most games, voice, Bedrock). |
| **Reverse proxy** | A server (Nginx/Caddy) that forwards web traffic to another app. |
| **Nameservers** | The DNS servers that answer for your domain (Cloudflare gives you two). |
| **MX / SPF / DKIM / DMARC** | Email routing & deliverability records. |
| **SSL / TLS** | The encryption around `https://` connections. |

---

<a name="templates"></a>
# 🧰 Ready-made templates

> Skip the copy-paste-from-the-guide step. These are real, ready-to-use config files — each heavily commented with `<PLACEHOLDERS>` to fill in.

| File | What it does | Grab it |
|:---|:---|:---|
| **Cloudflare Tunnel** config | Routes `http://` (websites/apps) **and** `tcp://` (games, SSH/RDP) | [`cloudflared/config.yml`](templates/cloudflared/config.yml) |
| **Nginx** reverse proxy | App on a custom port → clean `https://` URL | [`nginx/app.conf`](templates/nginx/app.conf) |
| **Caddy** reverse proxy | The 2-line, auto-HTTPS alternative | [`Caddyfile`](templates/Caddyfile) |
| **Docker Compose** | App + Nginx + cloudflared together | [`docker-compose.yml`](templates/docker-compose.yml) |

> 💡 Head to the [`templates/`](templates/README.md) folder for a short "how to run each one" guide.

---

<a name="further-reading"></a>
# 📚 Further reading

- [Cloudflare Docs — Add a site](https://developers.cloudflare.com/fundamentals/setup/add-site/)
- [Cloudflare Docs — Managed DNS / Records](https://developers.cloudflare.com/dns/manage-dns-records/)
- [Cloudflare Docs — Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)
- [Cloudflare Docs — Arbitrary TCP](https://developers.cloudflare.com/cloudflare-one/access-controls/applications/non-http/cloudflared-authentication/arbitrary-tcp/)
- [Cloudflare Community — DNS records for a Minecraft server](https://community.cloudflare.com/t/creating-dns-records-in-cloudflare-for-a-minecraft-server/176742)
- [Cloudflare Community — Setting up Minecraft SRV records](https://community.cloudflare.com/t/setting-up-minecraft-srv-records-on-cloudflare-connect-via-your-domain-name/636757)
- [Porkbun KB — SRV records for Minecraft](https://kb.porkbun.com/article/148-how-to-connect-your-domain-to-minecraft-using-srv-records)
- [Minecraft Wiki — Setting up a server](https://minecraft.wiki/w/Tutorials/Setting_up_a_server)

---

<a name="license"></a>
## 📄 License & Contributing

This guide is released under the **MIT License** — free to use, copy, and adapt. A ⭐ and a 👏 mean the world.

- 🐛 **Found an error?** Open an [issue](https://github.com/YOUR_USERNAME/cloudflare-domain-playbook/issues).
- 🛠️ **Improved a step?** Send a [pull request](https://github.com/YOUR_USERNAME/cloudflare-domain-playbook/pulls).
- 🙏 **Liked it?** Star the repo and share it with a friend.

> ⚠️ Replace `YOUR_USERNAME` in the links above with your GitHub handle.

---

<div align="center">

<br />

**✨ Done! You now have a Cloudflare-backed domain serving your website, your game, your apps, and even your email — all behind one clean domain.**

If this guide helped you, give it a ⭐! Found something wrong or confusing? Open a PR or issue. 🚀

</div>
