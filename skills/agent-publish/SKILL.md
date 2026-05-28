---
name: agent-publish
description: |
  Publish (distill) an OpenClaw agent to a public GitHub repository. 
  Strips private memory, de-identifies personal info, generates workspace 
  templates, and pushes a clean, reusable framework. Use when asked to 
  "publish an agent", "distill an agent", "打包 agent", "发布 agent to GitHub", 
  "把 agent 开源", or "extract an agent's framework".
triggers:
  - "publish agent"
  - "distill agent"
  - "打包 agent"
  - "发布 agent"
  - "把 agent 开源"
  - "extract agent framework"
  - "agent 发布"
  - "agent 蒸馏"
channel: any
status: ready
inputs:
  - source_workspace        # Path to agent workspace
  - github_repo              # e.g. moltpany/Agent-Name
  - agent_display_name       # Human-readable name (optional)
outputs:
  - published_repo           # GitHub URL
  - git_log                  # Commit history
---

# Agent Publish (Distill) Skill

Publish an OpenClaw agent as a reusable, public framework on GitHub.

## When to Use

User says any variant of:
- "publish / distill / package / 打包 / 发布 an agent"
- "extract an agent's framework to GitHub"
- "把 agent 开源"
- "把 agent 技能发布到 GitHub"

## Workflow Overview

### Phase 1: Discovery 🔍
1. List the source workspace: `find <workspace> -maxdepth 3 -type f`
2. Identify all skills under `<workspace>/skills/`
3. Read core workspace files: `IDENTITY.md`, `SOUL.md`, `AGENTS.md`, `TOOLS.md`, `USER.md`, `HEARTBEAT.md`
4. Note any private data that must be stripped

### Phase 2: De-identification & Templating 🧼

**Strip the following:**

| Category | Examples | Action |
|----------|----------|--------|
| **Real names** | "龙虾饲养员", "奇奇" | Replace with `_(user name)_` or remove |
| **Locations** | "浙江省宁波市", "广州番禺区" | Replace with `_(region)_` |
| **Equipment** | "松下 ZS99" | Replace with `_(camera model)_` |
| **App credentials** | App ID, App Secret, tokens | **NEVER include** |
| **Private memories** | `memory/` contents, `MEMORY.md` | **Exclude entirely** |
| **Runtime data** | `bird-records/`, `.openclaw/`, logs | **Exclude entirely** |
| **Personal photos** | `.jpg`, `.png` in records | **Exclude entirely** |

**Generate templates from real files:**
- Copy each core file to `references/<NAME>.md.template`
- Replace personal values with placeholders: `_(to be filled in)_`
- Keep structure, workflow, and expertise intact

### Phase 3: Build Repository 📦

Create the publish directory (e.g. `/tmp/agent-<name>-publish/`):

```
agent-<name>/
├── README.md                          ← Project landing page
├── LICENSE                            ← MIT (owner = repo owner)
├── .gitignore                         ← Exclude runtime data
├── skills/
│   └── <skill-name>/
│       └── SKILL.md                   ← Core skill(s) — verbatim copy
└── references/
    ├── IDENTITY.md.template
    ├── SOUL.md.template
    ├── AGENTS.md.template
    ├── TOOLS.md.template
    ├── USER.md.template
    └── HEARTBEAT.md.template
```

**README.md must include:**
- Project name + one-line pitch
- "What Is This?" section
- Core workflow / trigger phrases
- Repository layout (two views: repo structure vs workspace-after-setup)
- Quick Start with copy-paste commands
- Required APIs (if any)
- License

**.gitignore minimum:**
```
.openclaw/
memory/
*.jpg
*.jpeg
*.png
*.mp4
*.mov
.env
config.json
secrets/
```

### Phase 4: GitHub Push 🚀

1. `cd /tmp/agent-<name>-publish && git init && git add .`
2. Commit with descriptive message (include "framework for OpenClaw")
3. Configure SSH key for GitHub push:
   ```bash
   # Check for existing key
   ls ~/.ssh/id_* 2>/dev/null
   # If no key, user must create one via GitHub settings
   # If key exists (e.g. ~/.ssh/github_hr_agent), configure:
   cat >> ~/.ssh/config << 'EOF'
   Host github.com
     HostName github.com
     User git
     IdentityFile ~/.ssh/github_hr_agent
     IdentitiesOnly yes
   EOF
   chmod 600 ~/.ssh/config
   ```
4. Set remote to SSH: `git remote add origin git@github.com:<owner>/<repo>.git`
5. `git push -u origin main`

**If remote already has commits** (e.g. user created repo via web):
- `git fetch origin`
- If unrelated histories: `git merge origin/main --allow-unrelated-histories`
- Resolve any conflicts (usually LICENSE), commit, push

### Phase 5: Merge Claude Improvements 🎨

After push, check for a `claude/*` branch:
```bash
git fetch origin
git branch -r | grep claude
```

If exists:
1. Review diff: `git diff main..origin/claude/<branch>`
2. Typical Claude improvements:
   - README clarity (repo layout vs workspace layout)
   - DRY: remove duplicated workflow from SOUL.md.template
   - Add version tag, quick-start completeness
   - Trim redundant content
3. Merge if valuable: `git merge origin/claude/<branch> --ff-only`
4. Push updated main

### Phase 6: Quality Gate (Optional) 🧪

Before finalizing the release, run `agent-evals` on the distilled agent:

```bash
# Evaluate against three-pillar rubric
# See skills/agent-evals/SKILL.md for full procedure
```

1. **Run fixture tasks** (3–5 scenarios) end-to-end
2. **Score three pillars**:
   - skill chain (0.35) — Can skills actually execute?
   - knowledge (0.35) — Are sources traceable and uncertain claims bounded?
   - portability (0.30) — Can it run under a different model or host?
3. **Verdict**:
   - `pass` (all pillars ≥ 4.0, total ≥ 4.0) → proceed to finalize
   - `conditional` (total ≥ 3.5, one pillar < 4.0) → warn user, ask "push anyway?"
   - `fail` (total < 3.5 or any pillar < 2.0) → block, show remediation plan
4. **Publish eval trace** to `evals/<agent>-<version>.md` (if in Moltpany cohort)

**Principle**: Honest low score > padded high score. If Agent-HR itself gets L1 on portability, it gets L1 with a plan.

## Post-Publish Checklist

- [ ] All skills copied verbatim into `skills/`
- [ ] All workspace files templated in `references/`
- [ ] No personal data leaked
- [ ] No runtime data (memory/, photos, .openclaw/) included
- [ ] README has clear repo layout + workspace layout
- [ ] Quick Start is copy-paste ready
- [ ] LICENSE set to repo owner
- [ ] .gitignore excludes sensitive/runtime paths
- [ ] Successfully pushed to GitHub main
- [ ] Claude optimization branch merged (if any)

## Example Commit Message

```
init: Bird Photography Recorder framework for OpenClaw

- Core skill: photo-to-archive pipeline with iNaturalist integration
- Workspace templates: IDENTITY, SOUL, AGENTS, TOOLS, USER, HEARTBEAT
- Automatic logging: daily journals + species profiles + stats tracking
- Future IP support: card drafts and field guide material
```

## Notes

- **Always do this as HR, never ask the agent to self-publish.** The agent lacks perspective to judge what should be public.
- **SSH is preferred over HTTPS** for unattended pushes (no interactive password prompt).
- **If the user already created the GitHub repo** (via web or Claude Code), expect a non-empty remote. Handle with `--allow-unrelated-histories`.
- **Keep SKILL.md as the single source of truth** for workflows. Don't duplicate its contents into SOUL.md.template.
