---
name: agent-onboarding
description: Onboard a new OpenClaw agent into the system. Use when creating, registering, or deploying a new agent with its own workspace, channel account (Feishu/Discord/etc.), and configuration. Covers the full lifecycle from collecting credentials to pairing approval. Triggers on phrases like "create a new agent", "onboard an agent", "deploy a bot", "register agent", "add a new agent".
---

# Agent Onboarding Skill

Guide for bringing a new agent into an OpenClaw deployment.

## Workflow Overview

1. **Collect Application Info** — Gather credentials and preferences from the sponsor
2. **Register in Config** — Add the agent to OpenClaw's configuration files
3. **Create Workspace** — Set up the agent's working directory with core files
4. **Equip Skills** — Assign relevant skills based on the agent's job
5. **Restart Gateway** — Reload the system to recognize the new agent
6. **Pair & Approve** — Complete channel pairing and approve the connection
7. **First Day** — Agent introduces itself and begins work

## Step-by-Step

### 1. Collect Application Info

Get these from the human sponsor:
- **Channel credentials**: App ID & App Secret (Feishu), Bot Token (Discord), etc.
- **Agent ID**: Unique identifier, e.g. `hr-agent`, `finance-bot`
- **Display Name**: Human-readable name
- **Workspace path**: Where files live, e.g. `/workspace-hr`
- **Default model**: Brain to use, e.g. `minimax/auto`, `kimi/kimi-code`
- **Channel binding**: Which chat/group this agent serves

⚠️ **Security**: Never log or share App Secrets in chat. Store only in secure config.

### 2. Register in OpenClaw Config

Edit the OpenClaw configuration (typically `~/.openclaw/config.yaml` or similar):

```yaml
agents:
  list:
    - id: <agent-id>
      name: <display-name>
      model: <default-model>
      workspace: <workspace-path>

channels:
  feishu:
    accounts:
      - id: <agent-id>
        app_id: <app-id>
        app_secret: <app-secret>
        # ... other channel-specific settings

bindings:
  - channel: feishu
    account: <agent-id>
    agent: <agent-id>
    # Optional: filter by chat_id for group-specific agents
```

Also create the workspace directory:
```bash
mkdir -p <workspace-path>
```

### 3. Create Workspace Files

In the agent's workspace directory, create these core files:

| File | Purpose |
|------|---------|
| `IDENTITY.md` | Name, creature, vibe, emoji, avatar |
| `SOUL.md` | Personality, boundaries, behavior guidelines |
| `USER.md` | Who they serve, context to remember |
| `AGENTS.md` | Workspace rules (copy from template) |
| `TOOLS.md` | Channel notes, skill preferences, local config |
| `HEARTBEAT.md` | Periodic checks the agent should run |

For detailed templates for each file, see [references/workspace-templates.md](references/workspace-templates.md).

### 4. Equip Skills

Based on the agent's job description, assign relevant skills:
- **Feishu**: calendar, task, docs, sheets, bitable, IM
- **Enterprise WeChat**: contact, schedule, docs, smartsheet, msg
- **General**: weather, web-fetch, healthcheck, clawhub
- **Custom**: Write a `SKILL.md` + scripts if needed

### 5. Restart Gateway

After config changes, restart the OpenClaw Gateway so the new agent is loaded:

```bash
openclaw gateway restart
```

Or equivalent command for your deployment.

### 6. Pair & Approve

For channel bots (e.g., Feishu):
1. Human sends a message to the bot in the target chat
2. This triggers a **pairing request** with a pairing code
3. The onboarding skill user (or designated approver) receives the code
4. Approve the pairing in OpenClaw (exact mechanism varies by deployment)
5. Agent can now receive and send messages

### 7. First Day Onboarding

- Agent wakes up and reads its workspace files
- Introduces itself in the relevant group chat
- Starts handling its assigned tasks

## Post-Onboarding

- **Version control**: Commit the new agent's workspace files
- **Documentation**: Update team docs with the new agent's capabilities
- **Iterative improvement**: Adjust skills, model, and personality based on real usage

## Troubleshooting

- **Agent not responding after restart**: Check Gateway logs for config errors
- **Pairing fails**: Verify App ID/Secret and channel permissions
- **Messages not routing**: Check `bindings` config matches chat/account IDs

For the complete original onboarding example that inspired this skill, see [references/workspace-templates.md](references/workspace-templates.md).
