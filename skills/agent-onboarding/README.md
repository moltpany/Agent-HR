# Agent Onboarding Skill

Channel-agnostic 7-step pipeline for deploying a new agent into an OpenClaw workspace.

## What It Does

Covers the full deployment lifecycle:
1. Collect credentials and preferences from the sponsor
2. Register the agent in OpenClaw config
3. Create the agent workspace (IDENTITY.md, SOUL.md, USER.md, AGENTS.md, TOOLS.md, HEARTBEAT.md)
4. Equip skills based on the agent's role
5. Restart the Gateway
6. Pair and approve the channel bot
7. First-day onboarding

## Supported Channels

Feishu, Discord, WeCom, WhatsApp, Telegram

## Usage

Say to your HR agent:
> "Create a new agent" / "Onboard a bot" / "Deploy an agent"

For Feishu-specific creation with expectation interview and talent search, use the [`feishu-agent-creator`](../feishu-agent-creator/) skill instead.

## Files

```
skills/agent-onboarding/
├── SKILL.md                      ← Main skill (step-by-step guide)
└── references/
    └── workspace-templates.md    ← Templates for IDENTITY.md, SOUL.md, etc.
```
