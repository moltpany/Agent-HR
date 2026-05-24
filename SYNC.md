# GitHub 同步目录

This folder contains the **HR-Agent Framework** ready for publishing to GitHub.

## 📁 Structure

```
github/
├── README.md                          ← Project homepage
├── skills/
│   ├── feishu-agent-creator/          ← Feishu-specific agent onboarding
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── workspace-templates.md
│   └── agent-onboarding/              ← Generic agent onboarding (channel-agnostic)
│       ├── SKILL.md
│       └── references/
│           └── workspace-templates.md
└── SYNC.md                            ← This file: sync instructions
```

## 🔄 How to Sync (双向同步)

### Option A: 从 Workspace → GitHub (发布更新)

当你修改了 workspace 中的 skill，想更新 GitHub 版本时：

```bash
# 1. 进入 workspace
cd ~/.openclaw/workspace-hr

# 2. 复制最新文件到 github/
# feishu-agent-creator
cp skills/feishu-agent-creator/SKILL.md github/skills/feishu-agent-creator/
cp skills/feishu-agent-creator/README.md github/skills/feishu-agent-creator/
cp skills/feishu-agent-creator/references/*.md github/skills/feishu-agent-creator/references/

# agent-onboarding (打包 skill 需先解压)
mkdir -p github/skills/agent-onboarding/references
cp skills/agent-onboarding/SKILL.md github/skills/agent-onboarding/ 2>/dev/null || \
  unzip -p skills/agent-onboarding.skill SKILL.md > github/skills/agent-onboarding/SKILL.md
cp skills/agent-onboarding/references/*.md github/skills/agent-onboarding/references/ 2>/dev/null || \
  unzip -p skills/agent-onboarding.skill 'references/*' > /dev/null 2>&1

# 3. （可选）推送
# cd github && git add . && git commit -m "update: ..." && git push
```

### Option B: 从 GitHub → Workspace (拉取更新)

```bash
# 1. 拉取最新代码
# cd github && git pull

# 2. 复制回 workspace
# feishu-agent-creator
cp github/skills/feishu-agent-creator/SKILL.md skills/feishu-agent-creator/
cp github/skills/feishu-agent-creator/README.md skills/feishu-agent-creator/
cp github/skills/feishu-agent-creator/references/*.md skills/feishu-agent-creator/references/

# agent-onboarding
cp github/skills/agent-onboarding/SKILL.md skills/agent-onboarding/
cp github/skills/agent-onboarding/references/*.md skills/agent-onboarding/references/

# 3. 验证是否正常工作
openclaw config validate
```

### Option C: 使用 Symbolic Link (开发模式)

如果你经常修改，可以直接 symlink：

```bash
# ⚠️ 注意：这会直接让 github/ 指向 workspace 的实时文件
# 适合本地开发，不适合多人协作

ln -s ~/.openclaw/workspace-hr/skills/feishu-agent-creator github/skills/feishu-agent-creator
```

## 🚀 首次发布到 GitHub

```bash
cd ~/.openclaw/workspace-hr/github

git init
git add .
git commit -m "init: HR-Agent Framework for OpenClaw"

# 创建 GitHub repo (网页或 gh CLI)
# gh repo create hr-agent-framework --public --description "🍊 Your AI team's recruiter, HRBP, and coach"

git remote add origin https://github.com/YOUR_USERNAME/hr-agent-framework.git
git push -u origin main
```

## 📝 发布前 Checklist

- [ ] `agent-onboarding` 通用入职技能已同步
- [ ] `feishu-agent-creator` SKILL.md 已更新到最新
- [ ] README.md 包含完整 Roadmap
- [ ] references/workspace-templates.md 已包含
- [ ] 移除任何敏感信息（App ID、Secret、token）
- [ ] 添加 LICENSE 文件（建议 MIT）
- [ ] 添加 .gitignore（忽略 sensitive configs）

## 🎯 未来自动化

TODO: 可以写一个脚本 `scripts/sync.sh` 自动处理双向同步：
- 检测哪个目录更新
- 自动复制变更
- 生成 commit message
- 可选自动 push

---
**当前状态**: 文件已就绪，等待初始化 git 仓库并推送 🍊
