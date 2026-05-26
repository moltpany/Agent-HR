# Syncing Skills Between Workspace and GitHub

If you develop your skills locally in an OpenClaw workspace and want to keep them in sync with this repo:

## Workspace → GitHub (publish updates)

```bash
cd ~/.openclaw/workspace-hr

# Copy updated skill files
cp skills/feishu-agent-creator/SKILL.md /path/to/agent-hr/skills/feishu-agent-creator/
cp skills/feishu-agent-creator/references/*.md /path/to/agent-hr/skills/feishu-agent-creator/references/

cp skills/agent-onboarding/SKILL.md /path/to/agent-hr/skills/agent-onboarding/
cp skills/agent-onboarding/references/*.md /path/to/agent-hr/skills/agent-onboarding/references/

# Commit and push
cd /path/to/agent-hr
git add . && git commit -m "update: sync skills from workspace" && git push
```

## GitHub → Workspace (pull updates)

```bash
cd /path/to/agent-hr && git pull

# Copy back to workspace
cp skills/feishu-agent-creator/SKILL.md ~/.openclaw/workspace-hr/skills/feishu-agent-creator/
cp skills/feishu-agent-creator/references/*.md ~/.openclaw/workspace-hr/skills/feishu-agent-creator/references/

cp skills/agent-onboarding/SKILL.md ~/.openclaw/workspace-hr/skills/agent-onboarding/
cp skills/agent-onboarding/references/*.md ~/.openclaw/workspace-hr/skills/agent-onboarding/references/
```

## Pre-publish Checklist

- [ ] No sensitive data (App ID, App Secret, tokens)
- [ ] All referenced files exist in the repo
- [ ] README roadmap reflects actual status
