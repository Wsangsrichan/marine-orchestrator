# Agent Routing Guide

Orchestrator picks the right agent based on task type. Understand the routing to predict what will happen — or override it explicitly.

---

## Routing Table

| Task Type | Agent | When |
|-----------|-------|------|
| API / backend logic | `marine-backend` | REST endpoints, data models, integrations |
| Dashboard / UI | `marine-frontend` | React components, CSS, PWA features |
| Prompt / AI provider | `marine-prompt-engineer` | Gemini/Claude prompts, thinkingBudget, 2-step pattern |
| Trade analysis, statistics | `marine-analyst` | decisions.jsonl, Wilson CI, Kelly criterion, n≥50 rule |
| SSH / deploy / prod | `marine-devops` | pm2, nginx, git pull on prod, journalctl |
| Code exploration | `marine-research` | Codebase read, Phase 0.5 recon |
| Architecture decisions | `marine-ai` | Tech selection, ADRs, system design |
| Blog / content / docs | `marine-content` | Posts, README, SEO, copy |
| Security review | `marine-security` | OWASP Top 10, secrets scan, input validation |
| Position / PnL logic | `marine-trading` | stop-loss, take-profit, positionManager, lotteryPositions |
| Full pipeline | `marine-pilot` | When orchestrator delegates an entire sub-mission |

---

## Parallel vs Sequential

### Parallel (independent tasks)

```
marine-backend  ─┐
marine-frontend ─┼─→ merge when both done
marine-devops   ─┘
```

Use when: tasks have no dependencies — one agent's output doesn't affect another's input.

**Example:**
```
/marine-orchestrator add take-profit + fix blog SEO

→ marine-trading  [parallel] → take-profit logic (bot-polymarket)
→ marine-content  [parallel] → SEO meta (blog-tomato)
→ both done → report
```

### Sequential (dependent tasks)

```
marine-analyst → marine-trading → marine-devops
```

Use when: later agent needs earlier agent's output or approval.

**Example:**
```
/marine-orchestrator take-profit with stats verification before trading

→ marine-analyst  [step 1] → verify n≥50 trades
  ↓ if approved
→ marine-trading  [step 2] → implement PnL gate
  ↓ human gate
→ marine-devops   [step 3] → deploy
```

---

## Hard Rules (Quality Gates)

These override routing — orchestrator enforces them always:

### marine-analyst rule
> n < 50 resolved trades → **"cannot conclude"** — no strategy or threshold changes

```
If task involves: confidence thresholds, edge calculation, stop-loss %
And n < 50 resolved trades:
→ STOP. Report: "Insufficient data. Need 50+ resolved trades."
→ Do NOT route to marine-trading for threshold changes
```

### marine-prompt-engineer rule
> Prompt changes require before/after action distribution check

```
Before any JSON prompt change:
→ marine-analyst checks current action distribution
→ marine-prompt-engineer makes change
→ marine-analyst checks new distribution
→ Only proceed if distribution looks healthy
```

### marine-devops rule
> All prod deploys require human gate (Phase 2)

```
No deploy without:
⚠️ CRITICAL DECISION format
Approve? (y/n)
```

---

## Explicit Override

You can name the agent explicitly:

```bash
/marine-orchestrator [marine-security] review the new take-profit code
/marine-orchestrator [marine-analyst] check current win rate
/marine-orchestrator [marine-devops] restart pm2 on prod
```

Orchestrator will use that agent specifically.
