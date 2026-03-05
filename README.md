# MachFive Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/Open_Standard-Agent_Skills-blue.svg)](https://agentskills.io)
[![MachFive](https://img.shields.io/badge/Powered_by-MachFive-orange.svg)](https://machfive.io)

Agent Skills that connect any AI coding agent to [MachFive](https://machfive.io) — generate hyper-personalized cold email sequences directly from your AI workflows.

Compatible with **Claude Code**, **OpenAI Codex CLI**, **Gemini CLI**, and any tool that supports the [Agent Skills open standard](https://agentskills.io).

---

## What Is This?

This repository contains [Agent Skills](https://agentskills.io) that teach your AI agent how to use the MachFive API. Instead of switching between your agent and a separate tool, your agent autonomously calls MachFive to list campaigns, generate personalized email sequences, manage batch jobs, and export results — all through natural conversation.

**This is not a prompt template.** It's a live API integration — the agent calls the MachFive platform and returns real generated emails for your actual leads.

---

## Skills

| Skill | Triggers | What It Does |
|-------|---------|--------------|
| [`cold-email`](./cold-email/) | Cold email, outreach sequences, lead prospecting, email campaigns | Connects to the MachFive API to generate personalized email sequences for individual leads or batches |

---

## Install

### Claude Code

**Via plugin marketplace (recommended):**
```
/plugin marketplace add Bluecraft-AI/machfive-skills
/plugin install cold-email@machfive-skills
```

**Manual — personal (all projects):**
```bash
git clone https://github.com/Bluecraft-AI/machfive-skills.git
cp -r machfive-skills/cold-email/skills/cold-email ~/.claude/skills/
```

**Manual — project (shared with your team via git):**
```bash
cp -r machfive-skills/cold-email/skills/cold-email .claude/skills/
```

### OpenAI Codex CLI

**Personal:**
```bash
git clone https://github.com/Bluecraft-AI/machfive-skills.git
cp -r machfive-skills/cold-email/skills/cold-email ~/.agents/skills/
```

**Project:**
```bash
cp -r machfive-skills/cold-email/skills/cold-email .agents/skills/
```

### Gemini CLI

**Personal:**
```bash
git clone https://github.com/Bluecraft-AI/machfive-skills.git
cp -r machfive-skills/cold-email/skills/cold-email ~/.gemini/skills/
```

**Project:**
```bash
cp -r machfive-skills/cold-email/skills/cold-email .gemini/skills/
```

### Universal (Codex CLI, Gemini CLI, VS Code Copilot, OpenClaw, Kiro, and others)

Most agent tools discover skills from a shared cross-platform directory:
```bash
git clone https://github.com/Bluecraft-AI/machfive-skills.git
cp -r machfive-skills/cold-email/skills/cold-email ~/.agents/skills/
```

---

## Prerequisites

You need a MachFive API key and at least one campaign set up.

**Step 1: Get your API key**

1. Log in at [app.machfive.io](https://app.machfive.io)
2. Go to **Settings → Integrations → API Keys**
3. Click **Create API Key** and copy it

**Step 2: Set your environment variable**

```bash
export MACHFIVE_API_KEY=your_api_key_here
```

Add it to your shell profile (`.zshrc`, `.bashrc`, etc.) to persist across sessions.

**Step 3: Create a campaign**

The skill requires a campaign to generate against. Create one at [app.machfive.io/campaigns](https://app.machfive.io/campaigns). If you haven't specified a campaign, the agent will automatically list your available campaigns and ask you to pick one.

---

## What Your Agent Can Do

Once installed, the skill activates automatically when you ask about cold email, outreach, or lead sequencing. No slash command needed.

**Single lead**
```
Generate a 3-email sequence for jane@acme.com — she's VP of Growth at Acme Corp.
Use my SaaS Founders campaign.
```

**Batch**
```
Here are 20 leads [paste list]. Submit them for batch generation
using my Marketing Agencies campaign.
```

**Check status**
```
What's the status of my latest batch?
```

**Export**
```
Export my completed batch as CSV.
```

**Browse campaigns**
```
Show me my MachFive campaigns.
```

---

## How It Works

The skill gives your agent full knowledge of the MachFive API:

| Endpoint | What the agent uses it for |
|----------|---------------------------|
| `GET /api/v1/campaigns` | Lists your campaigns so you can pick one |
| `POST /api/v1/campaigns/{id}/generate` | Generates a sequence for one lead (sync) |
| `POST /api/v1/campaigns/{id}/generate-batch` | Submits multiple leads for async processing |
| `GET /api/v1/lists` | Browses your lead lists and batch jobs |
| `GET /api/v1/lists/{id}` | Polls batch status until complete |
| `GET /api/v1/lists/{id}/export` | Downloads results as CSV or JSON |

The agent handles the full workflow autonomously, including polling for batch completion.

---

## MachFive MCP Server

If you prefer MCP (Model Context Protocol) over Agent Skills, MachFive also has a dedicated MCP server that provides the same capabilities as structured tools:

👉 [github.com/Bluecraft-AI/machfive-mcp](https://github.com/Bluecraft-AI/machfive-mcp)

The MCP server works with Claude Desktop, Cursor, n8n, and any MCP-compatible client.

---

## Repository Structure

```
machfive-skills/
├── .claude-plugin/
│   └── marketplace.json          ← Claude Code marketplace catalog
├── cold-email/                   ← Claude Code plugin
│   ├── .claude-plugin/
│   │   └── plugin.json           ← plugin identity & metadata
│   └── skills/
│       └── cold-email/
│           └── SKILL.md          ← agent instructions (all platforms)
├── LICENSE
└── README.md
```

The `skills/cold-email/SKILL.md` file is the cross-platform skill that works with Claude Code, Codex CLI, Gemini CLI, and any Agent Skills-compatible tool. The `.claude-plugin/` directories are Claude Code-specific metadata for the plugin marketplace.

---

## Pricing

MachFive uses a credit-based model (1 credit = 1 lead processed):

| Plan | Credits/Month |
|------|--------------|
| Free | 100 |
| Starter | 2,000 |
| Growth | 5,000 |
| Enterprise | Custom |

The skill itself is free. Credits are consumed by the MachFive platform when generating emails.

---

## License

MIT — see [LICENSE](./LICENSE) for details.

---

## About MachFive

[MachFive](https://machfive.io) is an AI-powered cold email generation platform. It researches prospects and crafts unique, personalized outreach — not templates. Built for founders, sales teams, and growth operators who want to scale personalized outreach without sacrificing quality.

- **Website:** [machfive.io](https://machfive.io)
- **App:** [app.machfive.io](https://app.machfive.io)
- **Support:** [support@machfive.io](mailto:support@machfive.io)
- **MCP Server:** [github.com/Bluecraft-AI/machfive-mcp](https://github.com/Bluecraft-AI/machfive-mcp)
