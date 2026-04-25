# Marine Orchestrator

> **"All units, move in formation."** — Coordinate the marine team across repos, sprints, and critical decisions.

Marine Orchestrator is the **command layer** above individual marine agents. While [`/marine-pilot`](docs/pilot-vs-orchestrator.md) handles a single task end-to-end, `/marine-orchestrator` coordinates **multiple agents in formation** — parallel or sequential, across one or many repos.

---

## What It Does

```
/marine-orchestrator <task>          # Coordinate agents for a multi-agent task
/marine-orchestrator sprint          # Sprint planning from open GitHub issues  
/marine-orchestrator standup         # Daily standup ceremony
/marine-orchestrator deploy <repo>   # Deploy with mandatory human gate
/marine-orchestrator worktree <feature> <repos>  # Parallel worktrees
```

---

## The Pipeline

```
Phase 0: Parse intent (task type, repos, risk, agents needed)
Phase 1: Route agents (sequential or parallel)
Phase 2: Human gate (if critical action)
Phase 3: Execute + track state
Phase 4: Report mission complete
```

---

## When to Use Each

| Situation | Use |
|-----------|-----|
| Single task, one repo | `/marine-pilot` |
| Multi-agent, one repo | `/marine-orchestrator` |
| Multi-repo feature | `/marine-orchestrator worktree` |
| Sprint planning | `/marine-orchestrator sprint` |
| Production deploy | `/marine-orchestrator deploy` |
| Daily standup | `/marine-orchestrator standup` |

→ [Full comparison: pilot vs orchestrator](docs/pilot-vs-orchestrator.md)

---

## Quick Example

```
/marine-orchestrator implement take-profit 50% with analytics verification
```

**Orchestrator routes:**
```
marine-trading   → implement PnL gate in positionManager.js
marine-analyst   → verify n≥50 trades, no threshold changes
marine-devops    → deploy to prod (after human gate)
```

→ [More use cases](docs/use-cases.md)

---

## Critical Decision Gate

Before any action affecting live capital, positions, or prod:

```
⚠️ CRITICAL DECISION: Deploy to production
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  git push + pm2 restart bot-polymarket
Why:   take-profit fix ready, tests passing
Risk:  brief downtime (~10s), affects live positions

Approve? (y/n):
```

**This gate is mandatory. Orchestrator never auto-approves.**

→ [Decision gate details](docs/decision-gate.md)

---

## Built From

Patterns learned from [haocomm-multi-agent](https://github.com/watcharap0ng/haocomm-multi-agent) — zero-config, conversation-driven multi-agent orchestration.
