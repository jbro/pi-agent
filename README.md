# Pi Agent Config (`pi-agent`)

Safety-focused config for the Pi coding agent.

This repo is for Pi users who want a clean default setup with:
- practical operating rules,
- model defaults, and
- reusable skills.

## What this repo contains

- `SYSTEM.md` — core behavior and safety guardrails.
- `settings.json` — default provider/model and enabled models.
- `skills/` — workflow skills Pi can use.
- `.gitignore` — tracks only intended config files.

## Prerequisites

- Pi installed
- Git installed
- Provider/model auth configured for Pi

## Install / Use this config

> Expected location: `~/.pi/agent`

### Fresh setup
```bash
mv ~/.pi/agent ~/.pi/agent.backup.$(date +%Y%m%d-%H%M%S) 2>/dev/null || true
git clone https://github.com/jbro/pi-agent ~/.pi/agent
```

### Existing clone
```bash
cd ~/.pi/agent
git pull --ff-only
```

## Verify it is active

Run:

```bash
pi
```

Then ask Pi to summarize:
- active rules from `SYSTEM.md`
- default model from `settings.json`
