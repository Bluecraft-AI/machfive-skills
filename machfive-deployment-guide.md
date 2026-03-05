# MachFive Skills — Deployment & Distribution Guide

> All claims verified against official Anthropic docs at `claude.com/docs/plugins/submit` and `code.claude.com/docs/en/plugins-reference`.

---

## How the two names relate

| Name | What it is |
|---|---|
| `machfive-skills` | The GitHub **repo** — also serves as the marketplace |
| `cold-email` | The **plugin folder** inside that repo |

The full GitHub path is `Bluecraft-AI/machfive-skills`, and inside it lives a `cold-email/` folder that is the actual plugin.

```
Bluecraft-AI/machfive-skills/     ← GitHub repo root (the marketplace)
├── .claude-plugin/
│   └── marketplace.json          ← repo-level marketplace catalog
└── cold-email/                   ← the plugin
    ├── .claude-plugin/
    │   └── plugin.json           ← plugin identity & metadata
    └── skills/
        └── cold-email/
            └── SKILL.md          ← your skill instructions
```

> **Important:** `commands/`, `agents/`, `skills/`, and `hooks/` must be at the **plugin root** level (`cold-email/`), NOT inside `.claude-plugin/`. Only `plugin.json` goes inside `.claude-plugin/`. This is confirmed in the official Anthropic plugin reference docs.

---

## Phase 1–5: Repo Setup

### Step 1 — Create the GitHub repo

Create a new **public** repo named `machfive-skills` under `Bluecraft-AI`.

```bash
# On your Mac
mkdir machfive-skills
cd machfive-skills
git init
```

### Step 2 — Create the marketplace manifest

```bash
mkdir .claude-plugin
```

`.claude-plugin/marketplace.json`:
```json
{
  "name": "machfive-skills",
  "owner": {
    "name": "Bluecraft-AI",
    "url": "https://github.com/Bluecraft-AI"
  },
  "plugins": [
    {
      "name": "cold-email",
      "source": "./cold-email",
      "description": "Generate hyper-personalized cold email sequences using the MachFive API"
    }
  ]
}
```

### Step 3 — Create the plugin folder and manifest

```bash
mkdir -p cold-email/.claude-plugin
```

`cold-email/.claude-plugin/plugin.json`:
```json
{
  "name": "cold-email",
  "version": "1.0.0",
  "description": "Generate hyper-personalized cold email sequences using the MachFive API. Real API integration — not prompt templates.",
  "author": {
    "name": "Bluecraft-AI",
    "url": "https://github.com/Bluecraft-AI"
  },
  "homepage": "https://machfive.io",
  "repository": "https://github.com/Bluecraft-AI/machfive-skills",
  "license": "MIT",
  "keywords": ["cold-email", "sales", "outreach", "machfive"]
}
```

### Step 4 — Create the skills folder and SKILL.md

```bash
mkdir -p cold-email/skills/cold-email
```

`cold-email/skills/cold-email/SKILL.md`:
```markdown
---
name: cold-email
description: Generate hyper-personalized cold email sequences using the MachFive API. Use when the user wants to create, run, or manage cold email campaigns or generate personalized outreach for leads.
---

# MachFive Cold Email

[your existing SKILL.md content goes here]
```

### Step 5 — Add README and LICENSE

`README.md` at repo root — see the separate platform-agnostic README we built.

`LICENSE` — MIT license text.

### Step 6 — Final structure check

```
machfive-skills/
├── .claude-plugin/
│   └── marketplace.json
├── cold-email/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── skills/
│       └── cold-email/
│           └── SKILL.md
├── README.md
└── LICENSE
```

### Step 7 — Push to GitHub

```bash
git add .
git commit -m "Initial plugin structure"
git branch -M main
git remote add origin https://github.com/Bluecraft-AI/machfive-skills.git
git push -u origin main
```

---

## Phase 6: Test locally with `--plugin-dir`

> **Confirmed real flag** — Anthropic docs say: "Use the `--plugin-dir` flag to test plugins during development. This loads your plugin directly without requiring installation."

```bash
# Run from inside the machfive-skills/ repo root
claude --plugin-dir ./cold-email
```

⚠️ The flag points to the **plugin folder** (`./cold-email`), NOT the repo root (`.`). Pointing it at the repo root will fail silently.

Test inside Claude Code:
```
/plugin
# Should show cold-email plugin loaded

/help
# Should show any commands from your plugin
```

---

## Phase 7: Submit to Anthropic Official Plugin Directory

> **Verified URLs** — fetched directly from `claude.com/docs/plugins/submit` this session.

**Submission forms:**
- Claude.ai: https://claude.ai/settings/plugins/submit
- Console: https://platform.claude.com/plugins/submit

**What to submit:** The `cold-email/` plugin folder — not the whole `machfive-skills` repo. Either:
- Upload a `.zip` of the `cold-email/` folder, OR
- Provide this GitHub link: `https://github.com/Bluecraft-AI/machfive-skills/tree/main/cold-email`

**What happens after:**
- Anthropic runs basic automated review before listing
- "Anthropic Verified" badge requires additional review — not guaranteed
- Once listed, users install with: `/plugin install cold-email@claude-plugin-directory`

---

## Phase 8: SkillsMP (Automatic Indexing)

SkillsMP auto-crawls public GitHub repos following the Agent Skills standard. No manual submission needed.

**Requirements:**
- Repo is public
- 2+ GitHub stars
- Valid `SKILL.md` with YAML frontmatter (`name` + `description`)
- GitHub topics include `agent-skills` or `claude-skills`

Check indexing at https://skillsmp.com after reaching 2 stars (allow 24–48h).

---

## Phase 9: agent-skills.md

> **Confirmed real site** — verified from their live site.

Go to https://agent-skills.md → click **Submit** in the header → paste:

```
https://github.com/Bluecraft-AI/machfive-skills/tree/main/cold-email/skills
```

---

## Phase 10: heilcheng/awesome-agent-skills

Open contributions welcome. Fork the repo, add to README.md:

```
Bluecraft-AI/machfive-skills - Generate hyper-personalized cold email sequences using the MachFive API. Real API integration — not prompt templates.
```

Submit a PR to: https://github.com/heilcheng/awesome-agent-skills

---

## Phase 11: VoltAgent/awesome-agent-skills — DO THIS LATER

> Their README explicitly states: "Please don't submit skills you created 3 hours ago. We're now focusing on community-adopted skills, especially those published by development teams and proven in real-world usage."

Come back to this after you have real downloads and users. The bar is Cloudflare, Stripe, Netlify level — proven real-world adoption.

---

## Master Checklist

- [ ] Create `machfive-skills` repo on GitHub (Bluecraft-AI)
- [ ] Create `.claude-plugin/marketplace.json` at repo root
- [ ] Create `cold-email/.claude-plugin/plugin.json`
- [ ] Move SKILL.md → `cold-email/skills/cold-email/SKILL.md`
- [ ] Add `README.md` and `LICENSE` to repo root
- [ ] Add GitHub topics (`agent-skills`, `claude-code`, `cold-email`)
- [ ] Push to GitHub main
- [ ] Test locally: `claude --plugin-dir ./cold-email` (from repo root)
- [ ] Test `/plugin marketplace add Bluecraft-AI/machfive-skills` in Claude Code
- [ ] Submit `cold-email/` plugin to Anthropic → `claude.ai/settings/plugins/submit`
- [ ] Get 2 GitHub stars → auto-indexed by SkillsMP
- [ ] Submit to agent-skills.md
- [ ] Submit PR to heilcheng/awesome-agent-skills
- [ ] **LATER** — VoltAgent after real traction

---

*Verified against official Anthropic documentation. Last updated March 2026.*
