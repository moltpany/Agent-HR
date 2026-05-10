---
name: feishu-agent-creator
description: Create and deploy a new Feishu bot agent in OpenClaw. Use when creating, registering, or deploying a new Feishu agent with its own workspace, app credentials, and configuration. Covers the full lifecycle from collecting Feishu app credentials to pairing approval. Includes agent talent search — scout existing agent templates and skills before building from scratch. Triggers on phrases like "create a feishu agent", "deploy a feishu bot", "register feishu agent", "add a new feishu bot", "onboard a feishu agent", "find an agent for".
---

# Feishu Agent Creator Skill

Guide for bringing a new agent into an OpenClaw deployment.

## Workflow Overview

### Phase 0: Understand User Expectations 🎯
Before searching or building, deeply understand what the user wants:
1. **Agent's job** — What tasks should it handle?
2. **Personality preference** — Professional? Playful? Quirky? Deadpan?
3. **Target audience** — Who will chat with this agent?
4. **Skills needed** — Calendar? Documents? Data? Web search?
5. **Inspiration** — Any characters, roles, or existing agents they admire?

Guide the user to articulate their vision. The clearer the expectation, the better the search results and final agent.

### Phase 1: Agent Search & Talent Discovery 🔍
Before building from scratch, scout the global agent marketplace:
1. **Search GitHub** — Look for existing agent personalities (e.g., agency-agents)
2. **Search ClawHub** — Check OpenClaw's skill registry
3. **Check Internal Roster** — Avoid duplicating existing agents
4. **Assess Match** — Determine: reuse / adapt / build-new

### Phase 2: Onboarding
5. **Collect Application Info** — Gather credentials and preferences from the sponsor
6. **Register in Config** — Add the agent to OpenClaw's configuration files
7. **Create Workspace** — Set up the agent's working directory with core files
8. **Equip Skills** — Assign relevant skills based on the agent's job
9. **Restart Gateway** — Reload the system to recognize the new agent
10. **Pair & Approve** — Complete channel pairing and approve the connection
11. **First Day** — Agent introduces itself and begins work

## Step-by-Step

### 0. Understand User Expectations (CRITICAL — Do This First)

Before searching GitHub or writing any code, you MUST understand what the user actually wants. Ask guiding questions:

#### 0.1 The Expectation Interview

Ask the user to describe their ideal agent:

| Question | Why It Matters | Example Answers |
|----------|---------------|-----------------|
| **"这个 agent 主要负责什么工作？"** | Determines core skills | "帮我记录拍鸟照片" / "管理项目进度" / "回答客服问题" |
| **"你希望它是什么性格？"** | Guides personality design | "温暖亲切" / "毒舌吐槽" / "严谨专业" / "幽默风趣" |
| **"有没有喜欢的角色/形象作为参考？"** | Inspiration for identity | "像钢铁侠那样靠谱" / "像哆啦A梦" / "像一位老教授" |
| **"谁会跟这个 agent 聊天？"** | Defines audience and tone | "我自己用" / "团队成员" / "客户" / "我和朋友奇奇" |
| **"它需要操作哪些工具？"** | Determines skill requirements | "飞书日历" / "查看文档" / "搜索网页" / "记录数据" |
| **"有没有现有 agent 你觉得不错的？"** | Reference for quality bar | "我觉得 HR agent 那样的就很好" |

#### 0.2 Document the Vision

After the interview, write a brief "Agent Design Brief":
```markdown
# Agent Design Brief: [Name]

## Role
[What it does]

## Personality
[Character traits, vibe, emoji]

## Audience
[Who chats with it]

## Skills Needed
[List of skills/tools]

## Inspiration
[Characters, existing agents, or archetypes]

## Success Criteria
[How do we know it's working?]
```

#### 0.3 Search with Purpose

Now that you know what the user wants, search with specific keywords:
- Instead of "find an agent" → "find a bird photography assistant agent"
- Instead of "search GitHub" → "search for wildlife/nature documentation skills"

---

### 1. Agent Search & Talent Discovery (Optional but Recommended)

Before creating a new agent from scratch, search for existing agent templates, skills, and implementations that might already solve the problem. This avoids reinventing the wheel and leverages battle-tested agent personalities.

#### 0.1 Search GitHub Agent Repositories

Primary target: [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents)
- Contains 100+ specialized agent personalities across Engineering, Design, Marketing, Sales, Product, etc.
- Each agent includes: identity, personality, core mission, workflows, deliverables, success metrics
- Can be adapted to OpenClaw by extracting identity/personality and building corresponding workspace files

Search approach:
```bash
# Use web_fetch to read the README and find relevant agent profiles
curl -sL "https://raw.githubusercontent.com/msitarzewski/agency-agents/main/README.md" | grep -i "<keyword>"
```

Match criteria:
- **Direct match** → Agent specialty aligns with requested role
- **Partial match** → Agent covers related domain, can be adapted
- **No match** → Build from scratch using workspace templates

#### 0.2 Search ClawHub Skills Registry

Check if OpenClaw skills already exist for the agent's intended domain:
```bash
clawhub search <keyword>
```

Relevant skill categories:
- `feishu-*` — Feishu/Lark integrations (calendar, task, docs, sheets, bitable)
- `wecom-*` — Enterprise WeChat integrations
- `weather`, `web-fetch`, `healthcheck` — General utilities
- Custom skills in `~/.openclaw/workspace-*/skills/`

#### 0.3 Check Internal Agent Roster

Query existing agents to avoid duplication:
```bash
openclaw agents list
```

Check:
- Is there an existing agent with similar responsibilities?
- Can the existing agent be extended with new skills instead?
- Should the new agent be a specialization of an existing one?

#### 0.4 Talent Assessment Matrix

After searching, present a recommendation:

| Option | When to Choose | Action |
|--------|---------------|--------|
| **Reuse** | Exact match found in agency-agents or internal roster | Copy workspace, adapt identity, rebrand |
| **Adapt** | Partial match — domain overlaps but needs customization | Merge multiple agent personalities, add custom skills |
| **Build New** | No match found or highly specialized requirement | Use workspace-templates.md, craft custom identity |

#### 0.5 Translation from Template to OpenClaw

If reusing from agency-agents or similar template repos:

1. **Extract identity & personality** → Write to `IDENTITY.md` and `SOUL.md`
2. **Extract workflows & deliverables** → Write to `AGENTS.md` workflow section
3. **Map skills** → Find or create OpenClaw skills that match the agent's toolset
4. **Adapt channel config** → Original templates may target Claude Code/Cursor; translate to OpenClaw's feishu/Discord/WeCom bindings
5. **Localize** → Adjust for Chinese context (飞书/企微/小红书) if needed

⚠️ **Important**: agency-agents templates are prompt-based personalities for Claude Code. They need translation to OpenClaw's workspace + skills + channel configuration model.

---

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

**Method A: Using CLI (recommended)**

```bash
openclaw agents add "<Display Name>" \
  --workspace <workspace-path> \
  --model <default-model> \
  --bind feishu:<agent-id> \
  --non-interactive \
  --json
```

⚠️ Note: `--workspace` must be an absolute path the process can write to. If it fails with permission denied, create the directory first:
```bash
mkdir -p <workspace-path>
```

**Method B: Manual config edit**

Edit `~/.openclaw/openclaw.json` and add three sections:

**① Add to `agents.list`:**
```json
{
  "id": "<agent-id>",
  "name": "<Display Name>",
  "workspace": "<workspace-path>",
  "model": {
    "primary": "<default-model>"
  }
}
```

**② Add Feishu account to `channels.feishu.accounts`:**
```json
{
  "<agent-id>": {
    "appId": "<app-id>",
    "appSecret": "<app-secret>",
    "botName": "<bot-name>",
    "allowFrom": ["*"],
    "dmPolicy": "allowlist"
  }
}
```

**③ Add binding to `bindings`:**
```json
{
  "agentId": "<agent-id>",
  "match": {
    "channel": "feishu",
    "accountId": "<agent-id>"
  }
}
```

Validate config after changes:
```bash
openclaw config validate
```

### 3. Create Workspace

```bash
mkdir -p <workspace-path>
```

Then create core workspace files. For detailed templates, see [references/workspace-templates.md](references/workspace-templates.md).

### 4. Equip Skills

Copy relevant skills to `<workspace-path>/skills/`:
- **Feishu**: calendar, task, docs, sheets, bitable, IM
- **Enterprise WeChat**: contact, schedule, docs, smartsheet, msg
- **General**: weather, web-fetch, healthcheck, clawhub
- **Custom**: Write a `SKILL.md` + scripts if needed

### 5. Restart Gateway

```bash
openclaw gateway restart
```

Or check status first:
```bash
openclaw gateway status
```

### 6. Configure Feishu App (required for Feishu bots)

First, create a Feishu app:
1. Go to **Feishu Open Platform**: https://open.feishu.cn/page/launcher?user_openid=...
2. Click **Create App** → **Custom App**
3. Note down the **App ID** and **App Secret**

Then configure the app:
1. **Event Subscription** → Select "Use long connection to receive events" → Add event `im.message.receive_v1`
2. **Permission Management** → Import required permissions (see feishu-permissions.md)
3. **App Capability > Bot** → Enable bot capability
4. **Version Management & Release** → Create version → Submit for release

### 7. Pair & Approve

1. Human sends a message to the bot in the target chat
2. This triggers a **pairing request** with a code like `PAIR-XXXXXX`
3. Approve the pairing:
```bash
openclaw pairing approve feishu <PAIR-CODE>
```
4. Agent can now receive and send messages

### 8. First Day Onboarding

- Agent wakes up and reads its workspace files
- Introduces itself in the relevant group chat
- Starts handling its assigned tasks

## Post-Onboarding

- **Version control**: Commit the new agent's workspace files
- **Documentation**: Update team docs with the new agent's capabilities
- **Iterative improvement**: Adjust skills, model, and personality based on real usage

## Agent Roster Management (HR-Agent Core Capability)

As the HR agent, maintain an internal directory of all deployed agents:

```markdown
# Agent Roster

| Agent ID | Name | Division | Specialty | Status | Workspace |
|----------|------|----------|-----------|--------|-----------|
| hr-agent | HR 🍊 | HR/Talent | Agent onboarding, talent search | Active | ~/workspace-hr |
| ... | ... | ... | ... | ... | ... |
```

This roster enables:
- **Duplicate detection** before creating new agents
- **Team planning** — see gaps in coverage
- **Reassignment** — move skills between agents as needs change

## Troubleshooting

- **Agent not responding after restart**: Check Gateway logs for config errors
- **Pairing fails**: Verify App ID/Secret and channel permissions
- **Messages not routing**: Check `bindings` config matches chat/account IDs
- **Agent search finds template but deployment fails**: Templates often need translation from their native format (Claude Code prompts → OpenClaw workspace)

For the complete original onboarding example that inspired this skill, see [references/workspace-templates.md](references/workspace-templates.md).
