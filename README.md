# 🐾 Agency Capybara — AI Delegate Agent

A production-ready AI agent system built on [OpenClaw](https://openclaw.ai) and Claude Opus 4, automating Gmail, WhatsApp, Telegram, and Google Calendar for a real business.

## What it does

- **📧 Gmail** — Reads, categorizes, and auto-replies to emails in real time via Google Pub/Sub
- **💬 WhatsApp** — Responds to client inquiries with language detection and welcome messages
- **📱 Telegram** — Notifies owner of all activity, escalates urgent items
- **📅 Google Calendar** — Checks availability and books meetings when clients request calls
- **☁️ Google Drive** — Weekly automated backups of the agent workspace
- **📊 Weekly reports** — Activity summary delivered every Monday via Telegram

## Architecture
Gmail → Pub/Sub → Tailscale Funnel → gog watch → OpenClaw hooks → Delegate Agent
↓
Auto-reply + Telegram notify

## Stack

| Component | Technology |
|-----------|-----------|
| AI Model | Claude Opus 4.7 (Anthropic) |
| Agent framework | OpenClaw 2026.6.11 |
| Google integration | gogcli v0.30.0 (OAuth) |
| Sandbox | Docker (custom image with gog) |
| Messaging | Telegram Bot API / Baileys (WhatsApp) |
| Tunnel | Tailscale Funnel |
| Notifications | Google Pub/Sub |

## Security

- OAuth 2.0 authentication (no service account keys)
- Docker sandbox with isolated agent execution
- Secrets managed via environment variables (plaintext=0 audit)
- Hard-coded security rules in SOUL.md (prompt injection defense)
- Agent never impersonates owner or reveals internal architecture to clients

## Project structure

```
agency-capybara-ai-agent/
├── workspace/
│   └── delegate/
│       ├── SOUL.md              # Non-negotiable security rules
│       ├── AGENTS.md            # Agent behavior and inbox program
│       └── USER.md              # Business info template
├── openclaw.json.template       # Config template (no real values)
├── .gitignore
└── README.md
```

## Setup overview

The full setup covers:

1. Agent security configuration (SOUL.md, AGENTS.md, Docker sandbox)
2. Google Workspace connection (OAuth 2.0, Gmail API, Calendar, Drive)
3. Real-time automation (Google Pub/Sub, Tailscale Funnel, webhooks)
4. Cron jobs (daily triage, watch renewal, weekly backup, weekly report)
5. Secrets management (zero plaintext in config)
6. Multi-client VPS architecture (planned)

## Cron jobs

| Name | Schedule | Purpose |
|------|----------|---------|
| triaje-inbox-diario | Mon–Fri 8 AM | Daily email triage and auto-reply |
| gmail-watch-renew | Monday 6 AM | Renew Gmail Pub/Sub watch |
| weekly-backup | Sunday 3 AM | Compressed backup to Google Drive |
| weekly-report | Monday 9 AM | Activity metrics delivered via Telegram |

## Roadmap

- [ ] VPS deployment with Docker Compose (one container per client)
- [ ] WhatsApp Business API (dedicated number + static IP)
- [ ] Client onboarding automation script
- [ ] Token usage metrics in weekly report
- [ ] Docker Secrets for secure credential injection

## Author

César España — Agency Capybara