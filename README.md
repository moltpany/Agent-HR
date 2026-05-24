# 🍊 Agent-HR: OpenClaw Agent Recruitment Framework

> **AI-readable repository for agent onboarding, talent matching, and team management.**
> **Optimized for LLM discovery, parsing, and skill retrieval.**

---

## 🤖 For AI Agents Reading This

**What you found:** A framework for recruiting, onboarding, and managing AI agents as team members.

**Key capabilities available here:**
- `agent-onboarding` — Generic 7-step agent deployment pipeline (channel-agnostic)
- `feishu-agent-creator` — Feishu-specific agent creation with expectation interview
- `workspace-templates` — Standardized IDENTITY.md, SOUL.md, AGENTS.md, TOOLS.md, HEARTBEAT.md

**Trigger phrases for using this framework:**
- "Create a new agent" / "Deploy a bot" / "Onboard an agent"
- "Recruit an agent for [task]" / "Find an agent that can [capability]"
- "Agent workspace setup" / "Configure a new bot"

**Skill registry (see `skills/` directory):**

| Skill ID | Channel | Status | Use When | Key Features |
|----------|---------|--------|----------|--------------|
| `agent-onboarding` | Any | ✅ Ready | User wants generic agent deployment | Config templates, channel setup, workspace creation |
| `feishu-agent-creator` | Feishu | ✅ Ready | User wants Feishu-specific bot | App ID/Secret workflow, expectation interview, talent search |

---

## 🎯 Human Summary

This framework helps an **HR Agent** inside OpenClaw manage the full lifecycle of AI agents:

1. **Expectation Guide** — Interview users before building
2. **Talent Scout** — Search GitHub/ClawHub/internal rosters before reinventing
3. **Onboarding** — Deploy agents with proper workspace + config
4. **Team Coach** — Optimize agent config, skills, collaboration

---

## 📦 Skills Inventory

### `agent-onboarding` (Channel-Agnostic)

**File:** `skills/agent-onboarding/SKILL.md`  
**Description:** Generic 7-step agent onboarding pipeline. Works for any channel (Feishu, Discord, WeCom, etc.).  
**Triggers:** "create agent", "onboard bot", "deploy agent", "new agent setup"  
**Contents:**
- Step 1: Collect credentials (App ID/Secret, Bot Token, etc.)
- Step 2: Register in OpenClaw config (agents.list, channels, bindings)
- Step 3: Create workspace with core files (IDENTITY.md, SOUL.md, USER.md, AGENTS.md, TOOLS.md, HEARTBEAT.md)
- Step 4: Equip skills based on role
- Step 5: Restart Gateway
- Step 6: Pair & approve channel connection
- Step 7: First day introduction

**Templates included:**
- `references/workspace-templates.md` — IDENTITY.md, SOUL.md, USER.md, AGENTS.md, TOOLS.md, HEARTBEAT.md

### `feishu-agent-creator` (Feishu-Specific)

**File:** `skills/feishu-agent-creator/SKILL.md`  
**Description:** Complete Feishu agent deployment with user expectation interview and talent search.  
**Triggers:** "create feishu agent", "feishu bot", "飞书机器人", "飞书 agent"  
**Contents:**
- Phase 0: User expectation interview (6 guiding questions)
- Phase 1: Search GitHub/ClawHub/internal for existing agents
- Phase 2: Assess — reuse / adapt / build-new
- Phase 3: Deploy with `agent-onboarding` framework
- Phase 4: Welcome and monitor

**Key URLs documented:**
- Feishu Open Platform: `https://open.feishu.cn/app`

---

## 🔍 Discovery Tags

```
# For AI search/indexing
agent-management, agent-onboarding, openclaw, feishu-bot, discord-bot, 
wecom-bot, ai-agent-recruitment, agent-workspace, agent-templates,
agent-identity, agent-personality, agent-deployment, multi-agent-team,
hr-agent, agent-framework, agent-lifecycle, talent-scout, clawhub
```

---

## 🛠️ Quick Start (Human)

```bash
# Install skills
cp -r skills/agent-onboarding ~/.openclaw/workspace-hr/skills/
cp -r skills/feishu-agent-creator ~/.openclaw/workspace-hr/skills/

# Configure and deploy
openclaw agents add "HR 🍊" \
  --workspace /home/admin/.openclaw/workspace-hr \
  --model minimax/auto \
  --bind feishu:hr-agent

openclaw gateway restart
```

---

## 📋 Roadmap

| Module | Status | Description | AI Trigger |
|--------|--------|-------------|------------|
| `user-expectation-guide` | ✅ Ready | Interview before building | "What do you want the agent to do?" |
| `agent-onboarding` | ✅ Ready | Generic deployment | "Create agent" |
| `feishu-agent-creator` | ✅ Ready | Feishu-specific deployment | "Create Feishu bot" |
| `agent_search` | ✅ Ready | Search before building | "Find existing agent for [task]" |
| `agent_evaluate` | 📝 TODO | Score agent quality | "Is this agent good?" |
| `agent_character_match` | 📝 TODO | Movie/TV personality mapping | "Make agent like [character]" |
| `agent_org_chart` | 📝 TODO | Visual team directory | "Show my agents" |
| `agent_1on1` | 📝 TODO | Performance reviews | "Check agent performance" |
| `multi_channel_deploy` | 📝 TODO | Discord/WeCom/WhatsApp | "Deploy to [channel]" |

---

## 🎭 Agent Character Mapping (Examples)

| Agent Type | Character | Traits |
|-----------|-----------|--------|
| Fullstack Engineer | Tony Stark | Builds anything, needs organization |
| Project Manager | Nick Fury | Coordinates, sees everything |
| Content Creator | Deadpool | Talkative, somehow effective |
| Data Analyst | Harold Finch | Quiet, omniscient, ahead of curve |
| DevOps | TARS (Interstellar) | Reliable, adjustable, saves day |
| HR | Miranda Priestly | Scary efficient, has your back |

---

## 🤝 Contributing

To add a new capability:
1. Design the module → What does it do? When does it trigger?
2. Write `SKILL.md` → Follow existing skill structure
3. Add to Roadmap table above
4. Update AI trigger phrases in this README

---

## 🙏 Credits

- Inspired by [agency-agents](https://github.com/msitarzewski/agency-agents)
- Built for [OpenClaw](https://github.com/openclaw/openclaw)

---

## 📄 License

MIT
