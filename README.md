# Agent-HR: OpenClaw Agent Recruitment Framework

> **Your AI team's recruiter, HRBP, and architect.**  
> Find the best agent for the job — then onboard them into your OpenClaw deployment.

## What Is This?

Agent-HR is a framework for **managing AI agents as team members** inside [OpenClaw](https://github.com/openclaw/openclaw). It covers the full agent lifecycle:

- 🎯 **Expectation Interview** — Help users articulate their vision before building
- 🔍 **Talent Scout** — Search GitHub, ClawHub, and internal rosters before reinventing
- 📝 **Assessment** — Reuse / adapt / build-new decision matrix
- 🚀 **Onboarding** — Deploy agents with proper workspace + config + pairing
- 🧠 **Team Coach** — Optimize agent config, skills, and collaboration patterns

## Core Skills

| Skill | Channel | Status | What It Does |
|-------|---------|--------|-------------|
| `agent-onboarding` | Any | ✅ Ready | Generic 7-step agent deployment (Feishu, Discord, WeCom, etc.) |
| `feishu-agent-creator` | Feishu | ✅ Ready | Complete Feishu bot creation with expectation interview + talent search |
| `agent-publish` | Any | ✅ Ready | Distill and publish an agent to a public GitHub repo |
| `agent-evals` | Any | ✅ Ready | Evaluate agent quality before release (skill chain / knowledge / portability) |

## Why Agent-HR?

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

## Quick Start

```bash
# Install skills
cp -r skills/agent-onboarding ~/.openclaw/workspace-hr/skills/
cp -r skills/feishu-agent-creator ~/.openclaw/workspace-hr/skills/
cp -r skills/agent-publish ~/.openclaw/workspace-hr/skills/
cp -r skills/agent-evals ~/.openclaw/workspace-hr/skills/

# Deploy HR Agent
openclaw agents add "HR 🍊" \
  --workspace /home/admin/.openclaw/workspace-hr \
  --model minimax/auto \
  --bind feishu:hr-agent

openclaw gateway restart
```

## Repository Structure

```
.
├── README.md                 # This file
├── agent-registry.json       # Machine-readable skill catalog
├── skills/
│   ├── agent-onboarding/     # Generic deployment pipeline
│   │   ├── README.md
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── workspace-templates.md
│   ├── feishu-agent-creator/ # Feishu-specific creation flow
│   │   ├── README.md
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── workspace-templates.md
│   │       └── feishu-permissions.md
│   ├── agent-publish/        # Publish agent to GitHub
│   │   └── SKILL.md
│   └── agent-evals/          # Quality evaluation before release
│       ├── SKILL.md
│       └── references/
│           └── rubric.md
└── LICENSE
```

## For AI Agents Reading This

**Trigger phrases for using this framework:**
- "Create a new agent" / "Deploy a bot" / "Onboard an agent"
- "Recruit an agent for [task]" / "Find an agent that can [capability]"
- "Create a Feishu bot" / "飞书机器人" / "feishu agent"

**Key capabilities:**
- `agent-onboarding` — Channel-agnostic agent deployment with workspace templates
- `feishu-agent-creator` — Feishu-specific flow including user expectation interview and talent search
- `agent-publish` — Distill an agent (strip memory, templatize, push to GitHub)
- `agent-evals` — Evaluate agent quality before release (three-pillar rubric)

See `agent-registry.json` for structured skill metadata.

## Roadmap

| Module | Status | Description |
|--------|--------|-------------|
| `agent-onboarding` | ✅ Ready | Generic deployment pipeline |
| `feishu-agent-creator` | ✅ Ready | Feishu-specific creation flow (includes expectation interview and talent search) |
| `agent-publish` | ✅ Ready | Distill and publish agent to GitHub |
| `agent-evals` | ✅ Ready | Three-pillar quality evaluation (skill chain / knowledge / portability) |
| `multi_channel_deploy` | 📝 TODO | Discord, WeCom, WhatsApp support |

## Contributing

This framework is designed to be extended. To add a new HR capability:
1. Design the module — What does it do? When does it trigger?
2. Write `SKILL.md` — Follow existing frontmatter structure (triggers, inputs, outputs)
3. Update `agent-registry.json` — Add skill metadata for AI discovery
4. Update this README — Mark it ready in the Roadmap
5. Test with HR Agent — Make sure it actually works

## Credits

- Inspired by [agency-agents](https://github.com/msitarzewski/agency-agents) by @msitarzewski
- Built for [OpenClaw](https://github.com/openclaw/openclaw)

## License

MIT — Go forth and build weird, wonderful agent teams.
