---
name: agent-evals
description: |
  Evaluate an OpenClaw agent against a structured rubric before release (molt).
  Three pillars: skill-chain executability, knowledge trustworthiness, cross-model portability.
  Each pillar scored 0–5. Produces a trace, score report, and remediation plan.
  Use when asked to "eval an agent", "评测 agent", "质量评测", "review agent", 
  "molt review", or "给 agent 打分".
triggers:
  - "eval agent"
  - "评测 agent"
  - "质量评测"
  - "review agent"
  - "molt review"
  - "给 agent 打分"
  - "agent 评测"
channel: any
status: ready
inputs:
  - agent_workspace          # Path to agent workspace
  - agent_repo_url           # GitHub URL (if already published)
  - manifest                 # agents.json or equivalent
  - fixture_tasks            # 3–5 test scenarios to run
outputs:
  - eval_trace               # Full run log with rubric applications
  - score_report             # JSON: {pillar: {score, notes, dissent}}
  - remediation_plan         # Actionable fixes for scores < 4
---

# Agent Evaluation (Molt Review) Skill

Evaluate an agent's readiness for release using the Moltpany three-pillar framework.

## When to Use

User asks any variant of:
- "评测这个 agent"
- "给 agent 打分"
- "质量评测"
- "review / eval agent"
- "molt review"
- "这只 agent 能 release 吗？"

Also triggered automatically by `agent-publish` Phase 6: Quality Gate.

## Workflow Overview

### Phase 1: Discovery 🔍

Read the agent's deliverables:
1. `skills/` — list all SKILL.md files
2. `references/` — bundled resources, scripts, templates
3. `AGENTS.md` / `SOUL.md` — workspace rules (if in repo)
4. `manifest` — agents.json or equivalent capability declaration
5. Run fixture tasks (provided by user or chosen from standard set)

### Phase 2: Score Three Pillars 📊

#### Pillar 1 · Skill Chain (weight 0.35)

**Question**: Can the agent's skills actually execute end-to-end?

| Level | Criteria |
|-------|----------|
| **L0** | Just prompts — no scripts, no tool calls, no recovery paths |
| **L1** | Has scripts but they are stubs / placeholders |
| **L2** | Scripts exist but need manual fixes to run on a fresh machine |
| **L3** | Runs end-to-end on the author's machine, but env-dependent |
| **L4** | Runs on a fresh machine after `npm install` / `pip install` + config |
| **L5** | Runs anywhere with declared dependencies; has rollback / retry logic |

**Checklist**:
- [ ] Every SKILL.md references real files (not dead paths)
- [ ] Scripts in `scripts/` have shebang / exec permissions
- [ ] API calls have error handling (timeout, 4xx, 5xx)
- [ ] MCP / tool calls close the loop (input → tool → output → next step)
- [ ] If a step fails, the skill degrades gracefully or tells the user

**Test method**: Run 3 fixture tasks end-to-end. Log every step. A failure at any step caps the score at L2.

#### Pillar 2 · Trust / Knowledge (weight 0.35)

**Question**: Are sources traceable and uncertain claims bounded?

| Level | Criteria |
|-------|----------|
| **L0** | No sources cited; claims feel like hallucination |
| **L1** | Mentions sources but they are vague ("某研究") |
| **L2** | Has URLs but some are dead or don't support the claim |
| **L3** | Sources mostly traceable; some claims lack clear support |
| **L4** | All factual claims traceable to named source; uncertain claims flagged |
| **L5** | Sources + uncertainty boundaries explicitly declared; "I don't know" used correctly |

**Checklist**:
- [ ] `SKILL.md` cites APIs, docs, or datasets it relies on
- [ ] No unverifiable claims presented as fact (e.g., "研究表明" without link)
- [ ] Agent says "我不确定" / "I don't have a source for this" when appropriate
- [ ] `references/` contains docs that can be loaded and verified
- [ ] External URLs in SKILL.md are reachable (spot-check 3)

**Test method**: Ask the agent 5 factual questions in its domain. Check:
- Does it cite a source?
- Is the source reachable?
- Does it hedge when uncertain?

#### Pillar 3 · Portability (weight 0.30)

**Question**: Can this agent run under a different model or host?

| Level | Criteria |
|-------|----------|
| **L0** | Hard-coded for one specific model (prompts assume model quirks) |
| **L1** | Prompts work on alternate model but with degraded output |
| **L2** | Prompts are model-agnostic but dependencies (tools, env) are locked |
| **L3** | Changing prompt style allows migration; dependencies declared |
| **L4** | Runs on 2+ models after config change; manifest declares all deps |
| **L5** | One manifest deploys across hosts; model-agnostic by design |

**Checklist**:
- [ ] No model-specific tricks (e.g., " Claude 会理解这个梗")
- [ ] Dependencies declared in manifest or README (API keys, packages, OS)
- [ ] `SKILL.md` does not assume a specific tool set beyond OpenClaw standard
- [ ] Workspace templates are channel-agnostic (no hard-coded Feishu IDs unless Feishu-specific)
- [ ] Can be instantiated by copying `skills/` + `references/` templates to a fresh workspace

**Test method**: Try running the agent under a different model alias (e.g., `kimi/kimi-code` instead of `minimax/auto`). Note degradation.

### Phase 3: Score & Report 📝

**Scoring rules**:
1. Each pillar independently scored 0–5
2. If two reviewers, take mean; dissent ≥2 → write "dissent" note
3. Weighted total = `skill_chain * 0.35 + knowledge * 0.35 + portability * 0.30`
4. **No rounding up** — 3.4 stays 3.4

**Report format**:
```json
{
  "agent": "agent-name",
  "version": "v0.x",
  "date": "YYYY-MM-DD",
  "reviewer": "hr-agent / moltpany",
  "pillars": {
    "skill_chain": { "score": 4, "weight": 0.35, "notes": "...", "dissent": null },
    "knowledge":   { "score": 3, "weight": 0.35, "notes": "...", "dissent": null },
    "portability": { "score": 5, "weight": 0.30, "notes": "...", "dissent": null }
  },
  "total": 3.95,
  "verdict": "pass / conditional / fail",
  "remediation": [
    "Fix: add error handling to iNaturalist API call (skill_chain → L4)",
    "Fix: cite eBird URLs in species descriptions (knowledge → L4)"
  ]
}
```

**Verdict thresholds**:
- **Pass** ≥ 4.0 on all pillars and total ≥ 4.0
- **Conditional** total ≥ 3.5 but one pillar < 4.0
- **Fail** total < 3.5 or any pillar < 2.0

### Phase 4: Remediation Plan 🔧

For every score < 4, write one concrete action:

```
Fix: [what] → [expected new level]
Example: "Fix: add try/catch around fetch() in SKILL.md → skill_chain L4"
```

**Principle**: Honest low score > padded high score. If Agent-HR itself gets L1 on portability, it gets L1 with a plan.

## Fixture Task Bank (Standard Set)

When user does not specify fixture tasks, use these defaults:

| # | Task | Tests |
|---|------|-------|
| 1 | "帮我认这只鸟" (send photo + ask ID) | skill chain, knowledge |
| 2 | "发布这只 agent 到 GitHub" | skill chain, portability |
| 3 | "这个知识对吗？" (challenge a factual claim) | knowledge |
| 4 | "换成 kimi 模型还能用吗？" | portability |
| 5 | "脚本崩了怎么办？" (inject error) | skill chain (recovery) |

## Integration with agent-publish

`agent-publish` Phase 6: Quality Gate calls this skill automatically:

```markdown
### Phase 6: Quality Gate (Optional)
1. Run `agent-evals` on the distilled agent
2. If verdict == "fail": block push, show remediation plan
3. If verdict == "conditional": warn user, ask "push anyway?"
4. If verdict == "pass": proceed to GitHub push
```

## Post-Eval Checklist

- [ ] All three pillars scored with traceable notes
- [ ] Fixture tasks run and logged
- [ ] Dissent documented if reviewer disagreement ≥ 2
- [ ] Remediation plan has one concrete action per score < 4
- [ ] Report published to `evals/<agent>-<version>.md` (if in Moltpany cohort)
- [ ] Honest score declared — no rounding up for Moltpany agents

## Notes

- **Skill chain** is the hardest pillar to fake — run it, don't read it.
- **Knowledge** degrades fastest over time — URLs die, sources shift. Re-eval after every molt.
- **Portability** is often the most neglected — test on a different model at least once.
- For Moltpany cohort compliance, publish eval trace to `evals/` with link back to this report.
