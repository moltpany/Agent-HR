# 🍊 HR-Agent Framework for OpenClaw

> **Your AI team's recruiter, HRBP, and coach.**  
> Find the best agent for the job — then onboard them into your OpenClaw deployment.

---

## 🎯 What Is This?

This is a framework for running an **HR Agent** inside OpenClaw. Its job is not just "create a bot" — it's the full lifecycle of managing AI agents as team members:

- 🎯 **Expectation Guide** — Help users articulate their vision before building
- 🔍 **Talent Scout** — Search GitHub, ClawHub, and internal rosters before building from scratch
- 📝 **HRBP** — Understand business needs and match the right agent type + personality
- 🚀 **Onboarding Specialist** — Deploy agents via `feishu-agent-creator` (or extend to Discord/WeCom)
- 🧠 **Team Coach** — Optimize agent config, skills, and collaboration patterns
- 🏗️ **Architect** — Design multi-agent teams that work together without overlap

---

## 🎨 Design Philosophy

### Agents Are Team Members, Not Tools

Every agent gets:
- **An identity** — Name, emoji, avatar, creature type
- **A personality** — Warm? Sharp? Snarky? Deadpan?
- **A role** — What they own, what they don't
- **A workspace** — Their own files, memory, tools
- **KPIs** — Response quality, task completion, user satisfaction

### Don't Reinvent the Wheel

Before building a new agent, search the global agent marketplace:
- [agency-agents](https://github.com/msitarzewski/agency-agents) — 100+ specialized AI personalities
- [ClawHub](https://clawhub.ai) — OpenClaw skill registry
- Internal roster — Agents already deployed in your org

### Fun Is a Feature

- **影视角色映射** — Match agent personalities to movie characters
- **表情包系统** — Each agent has their own sticker/meme library
- **入职欢迎仪式** — New agents introduce themselves with a bit of theater
- **1:1 绩效面谈** — Regular check-ins: "How's your month going? Need new skills?"

---

## 📦 Core Modules

| Module | Status | Description |
|--------|--------|-------------|
| `user-expectation-guide` | ✅ Ready | Interview users to articulate agent vision before searching/building |
| `feishu-agent-creator` | ✅ Ready | Full agent onboarding pipeline for Feishu bots |
| `agent_search` | ✅ Ready | Search GitHub/ClawHub/internal before building |
| `agent_evaluate` | 📝 TODO | Score agent quality, match to requirements |
| `agent_character_match` | 📝 TODO | Map agent personalities to movie/TV characters |
| `agent_org_chart` | 📝 TODO | Maintain visual org chart of all agents |
| `agent_1on1` | 📝 TODO | Periodic performance check-ins with agents |
| `agent_reassignment` | 📝 TODO | Move skills/personas between agents |
| `multi_channel_deploy` | 📝 TODO | Extend onboarding to Discord, WeCom, WhatsApp |

---

## 🚀 Quick Start

### Deploy the HR Agent

```bash
# 1. Install the skill
cp -r skills/feishu-agent-creator ~/.openclaw/workspace-hr/skills/

# 2. Configure OpenClaw
openclaw agents add "HR 🍊" \
  --workspace /home/admin/.openclaw/workspace-hr \
  --model minimax/auto \
  --bind feishu:hr-agent

# 3. Restart gateway
openclaw gateway restart

# 4. Pair the bot
# Send a message to the HR bot in Feishu → get PAIR code
openclaw pairing approve feishu PAIR-XXXXXX
```

### Create a New Agent (via HR)

Say to HR Agent:
> "帮我招一个前端开发的 agent"

HR Agent will:
1. 🎯 **Conduct expectation interview** — "你希望它是什么性格？" "需要哪些技能？"
2. 🔍 Search [agency-agents](https://github.com/msitarzewski/agency-agents) for Frontend Developer
3. 📝 Check internal roster for duplicates
4. 🎯 Assess: reuse / adapt / build-new
5. 🚀 Run onboarding if building new
6. 🎉 Welcome the new agent to the team

---

## 🎭 Agent Character Mapping (Examples)

| Agent Type | Movie Character | Why |
|-----------|-----------------|-----|
| Fullstack Engineer | Tony Stark (Iron Man) | Builds anything, needs JARVIS to keep him organized |
| Project Manager | Nick Fury (Avengers) | Sees everything, coordinates the impossible |
| Content Creator | Deadpool | Never shuts up, but somehow it works |
| Data Analyst | Harold Finch (Person of Interest) | Quiet, omniscient, always three steps ahead |
| DevOps | TARS (Interstellar) | Reliable, adjustable humor setting, saves the day |
| HR (this agent) | Miranda Priestly (Devil Wears Prada) | Scary efficient, but has your back |

---

## 📋 Roadmap / TODO

### Phase 1: Foundation ✅
- [x] `user-expectation-guide` — Interview framework to articulate agent vision
- [x] `feishu-agent-creator` — Full onboarding pipeline
- [x] `agent_search` — GitHub + ClawHub + internal roster search
- [x] Workspace templates — IDENTITY.md, SOUL.md, AGENTS.md, TOOLS.md
- [x] Talent assessment matrix — reuse / adapt / build-new
- [x] Feishu Open Platform URL documented — https://open.feishu.cn/app

### Phase 2: Intelligence 🚧
- [ ] `agent_evaluate` — Automated scoring of agent quality
  - Response accuracy
  - Task completion rate
  - User satisfaction tracking
  - Skill coverage analysis
- [ ] `agent_character_match` — Movie/TV character personality mapping
  - Character database
  - Personality quiz for new agents
  - Meme/sticker assignment
- [ ] `agent_org_chart` — Visual team directory
  - Mermaid/graphviz org chart generation
  - Agent relationship mapping (who calls whom)
  - Skill overlap detection

### Phase 3: Management 📝
- [ ] `agent_1on1` — Performance review system
  - Monthly check-in prompts
  - Skill gap identification
  - Training recommendations
- [ ] `agent_reassignment` — Team reorganization
  - Move skills between agents
  - Merge overlapping agents
  - Archive retired agents
- [ ] `agent_succession` — Backup agent planning
  - Cross-training skills
  - Failover agent activation

### Phase 4: Scale 📈
- [ ] `multi_channel_deploy` — Beyond Feishu
  - Discord bot onboarding
  - WeCom (企业微信) agent creation
  - WhatsApp/Telegram support
- [ ] `agent_marketplace` — Internal agent store
  - Browse pre-built agents
  - One-click deployment
  - Rating/review system
- [ ] `agent_collaboration` — Multi-agent workflows
  - Handoff protocols between agents
  - Shared memory / context passing
  - Team meeting simulation

### Phase 5: Fun 🎉
- [ ] `agent_emojis` — Automatic emoji/sticker generation
  - DALL-E/Midjourney integration for avatars
  - Meme template matching
- [ ] `agent_lore` — Backstory generation
  - Origin story for each agent
  - Team mythology building
- [ ] `agent_drama` — Inter-agent dynamics (just for fun)
  - Friendly rivalries
  - Agent "birthdays"
  - Team building events

---

## 🛠️ Skill Structure

```
skills/feishu-agent-creator/
├── SKILL.md                          ← Main skill file
├── references/
│   └── workspace-templates.md        ← IDENTITY.md, SOUL.md templates
└── README.md                         ← This file (framework overview)
```

---

## 🤝 Contributing

This framework is designed to be extended. To add a new HR capability:

1. **Design the module** — What does it do? When does it trigger?
2. **Write the skill** — Add to `SKILL.md` or create a sub-skill
3. **Update the README** — Mark it ready in the Roadmap
4. **Test with HR Agent** — Make sure it actually works

---

## 🙏 Credits

- Inspired by [agency-agents](https://github.com/msitarzewski/agency-agents) by @msitarzewski
- Built for [OpenClaw](https://github.com/openclaw/openclaw)
- HR Agent persona designed by @ou_441ba2d680520a31eae6227b561df669 🍊🦞

---

## 📄 License

MIT — Go forth and build weird, wonderful agent teams.
