# Quick Start

## Setup

`/marine-orchestrator` is a Claude Code skill — no install needed, just invoke it.

```bash
# In any Claude Code session:
/marine-orchestrator <task>
```

The skill file lives at `~/.claude/skills/marine-orchestrator/SKILL.md`. Claude Code loads it automatically.

---

## Your First Mission

### 1. Standup (no agents, instant)

```bash
/marine-orchestrator standup
```

Outputs: done yesterday, today's plan, blockers, bot status.

### 2. Sprint Planning

```bash
/marine-orchestrator sprint
```

Lists open GitHub issues, estimates size, assigns agents, defines done criteria.

### 3. Multi-Agent Task

```bash
/marine-orchestrator implement <feature> with <verification>
```

Orchestrator routes agents, tracks state, enforces gates.

### 4. Deploy with Gate

```bash
/marine-orchestrator deploy bot-polymarket
```

Always shows critical decision gate. You approve → marine-devops executes.

---

## First Decision: pilot or orchestrator?

```
Is it a single task in one repo?
→ /marine-pilot <task>

Does it need 2+ agents, multiple repos, ceremonies, or a deploy gate?
→ /marine-orchestrator <task>
```

Still unsure? Start with `/marine-pilot`. If you find yourself coordinating multiple agents manually, upgrade to `/marine-orchestrator`.

→ [Full comparison](pilot-vs-orchestrator.md)

---

## State File (optional)

For long missions, orchestrator tracks state:

```json
// .marine/orchestrator-state.json
{
  "mission": "take-profit-50",
  "github_issue": "#10",
  "phase": "execute",
  "agents": {
    "marine-trading": "done",
    "marine-analyst": "done",
    "marine-devops": "pending"
  },
  "worktrees": []
}
```

This file is gitignored — local state only.
