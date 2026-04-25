# Command Reference

## Core Commands

```bash
/marine-orchestrator <task>
```
Coordinate agents for a multi-agent task. Orchestrator parses intent, routes agents, enforces gates.

---

```bash
/marine-orchestrator sprint
```
Sprint planning from open GitHub issues. Estimates size (S/M/L), assigns agents, defines done criteria, sets checkpoint schedule.

---

```bash
/marine-orchestrator standup
```
Daily standup ceremony. Reads git history + handoffs + open issues → formats standup output.

---

```bash
/marine-orchestrator deploy <repo>
```
Deploy with mandatory critical decision gate. Triggers:
1. Gate format (What/Why/Risk + Approve?)
2. After approval → marine-devops executes
3. Post-deploy verification

---

```bash
/marine-orchestrator worktree <feature> <repo1> [repo2] ...
```
Create isolated git worktrees for parallel development across repos. Assigns agents to each worktree. Coordinates merge and cleanup.

---

## Ceremonial Commands

```bash
/marine-orchestrator sprint review    # Review what shipped vs planned
/marine-orchestrator retro            # Quick agent + gate retrospective
/marine-orchestrator sprint retro     # Combined review + retro
```

---

## Explicit Agent Override

```bash
/marine-orchestrator [marine-security] review new code in positionManager.js
/marine-orchestrator [marine-analyst] check current win rate and trade count
/marine-orchestrator [marine-devops] check pm2 status on prod
```

Brackets `[agent-name]` force a specific agent instead of auto-routing.

---

## Resume

```bash
/marine-orchestrator resume
```
Resume a paused mission from `.marine/orchestrator-state.json`.

---

## Flags

| Flag | Effect |
|------|--------|
| (none) | Full pipeline with all gates |
| Task description with agent name in `[brackets]` | Force specific agent |

---

## RTK (Token Optimization)

All bash commands inside orchestrator use `rtk` prefix:

```bash
rtk git status       # not: git status
rtk ls /root/bot-polymarket
rtk git log --oneline -5
```

Skip `rtk` only for: `npm install`, `node -e`, `npx`, `ssh`, `gh`, `git push`
