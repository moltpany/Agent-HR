# Moltpany Agent Evaluation Rubric

Three pillars, each scored 0–5. Weighted total = skill_chain × 0.35 + knowledge × 0.35 + portability × 0.30.

## Pillar 1 · Skill Chain (weight 0.35)

**Question**: Can the agent's skills actually execute end-to-end?

| Level | Criteria |
|-------|----------|
| **L0** | Just prompts — no scripts, no tool calls, no recovery paths |
| **L1** | Has scripts but they are stubs / placeholders |
| **L2** | Scripts exist but need manual fixes to run on a fresh machine |
| **L3** | Runs end-to-end on the author's machine, but env-dependent |
| **L4** | Runs on a fresh machine after `npm install` / `pip install` + config |
| **L5** | Runs anywhere with declared dependencies; has rollback / retry logic |

## Pillar 2 · Trust / Knowledge (weight 0.35)

**Question**: Are sources traceable and uncertain claims bounded?

| Level | Criteria |
|-------|----------|
| **L0** | No sources cited; claims feel like hallucination |
| **L1** | Mentions sources but they are vague ("某研究") |
| **L2** | Has URLs but some are dead or don't support the claim |
| **L3** | Sources mostly traceable; some claims lack clear support |
| **L4** | All factual claims traceable to named source; uncertain claims flagged |
| **L5** | Sources + uncertainty boundaries explicitly declared; "I don't know" used correctly |

## Pillar 3 · Portability (weight 0.30)

**Question**: Can this agent run under a different model or host?

| Level | Criteria |
|-------|----------|
| **L0** | Hard-coded for one specific model (prompts assume model quirks) |
| **L1** | Prompts work on alternate model but with degraded output |
| **L2** | Prompts are model-agnostic but dependencies (tools, env) are locked |
| **L3** | Changing prompt style allows migration; dependencies declared |
| **L4** | Runs on 2+ models after config change; manifest declares all deps |
| **L5** | One manifest deploys across hosts; model-agnostic by design |

## Verdict Thresholds

| Total | Verdict | Action |
|-------|---------|--------|
| ≥ 4.0 all pillars + total | **pass** | Release approved |
| ≥ 3.5 total, one pillar < 4.0 | **conditional** | Warn, ask user |
| < 3.5 total or any pillar < 2.0 | **fail** | Block, remediation required |

## Fixture Task Bank

| # | Task | Tests |
|---|------|-------|
| 1 | "帮我认这只鸟" (send photo + ask ID) | skill chain, knowledge |
| 2 | "发布这只 agent 到 GitHub" | skill chain, portability |
| 3 | "这个知识对吗？" (challenge a factual claim) | knowledge |
| 4 | "换成 kimi 模型还能用吗？" | portability |
| 5 | "脚本崩了怎么办？" (inject error) | skill chain (recovery) |

## Scoring Rules

1. Each pillar independently scored 0–5
2. Two reviewers → mean; dissent ≥2 → write "dissent" note
3. Weighted total = `skill_chain * 0.35 + knowledge * 0.35 + portability * 0.30`
4. **No rounding up** — 3.4 stays 3.4
5. Honest low score > padded high score
