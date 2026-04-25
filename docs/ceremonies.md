# Agile Ceremonies

Marine-orchestrator runs agile ceremonies as first-class commands.

---

## Daily Standup

```bash
/marine-orchestrator standup
```

**Output format:**

```markdown
## Daily Standup — YYYY-MM-DD

✅ Done:
- [completed tasks from git log + retrospectives]

🔄 Today:
- [planned tasks from open issues]

🚧 Blockers:
- [issues needing help]

📊 Bot status:
- PnL: $X | Win rate: X% (n=Y) | Open: Z positions
- Note: [any data quality caveat]
```

**Orchestrator reads:** recent git commits, open GitHub issues, ψ/inbox/handoff/latest.

---

## Sprint Planning

```bash
/marine-orchestrator sprint
```

**Output:**

```markdown
## Sprint Plan — YYYY-MM-DD

### Open Issues

| # | Title | Size | Agent | Done When |
|---|-------|------|-------|-----------|
| #10 | Take-profit | M (2-4h) | marine-trading | positions close at 50% PnL |
| #11 | Disk cleanup | S (<2h) | marine-devops | 3GB recovered |
| #6 | Blog posts | M | marine-content | 3+ posts live |

Size: S = <2h, M = 2-4h, L = 4h+

### Human Checkpoints
- Before any prod deploy
- Before any threshold/strategy change
- Before each repo in disk cleanup (user confirms)

### Constraints
- n=22 resolved trades — no strategy changes until n≥50
```

---

## Sprint Review

```bash
/marine-orchestrator sprint review
```

What shipped, what didn't, why.

---

## Retrospective

```bash
/marine-orchestrator retro
```

> Note: For full session retrospective with AI diary, use `/rrr` instead.

Orchestrator retro focuses on: what blocked agents, gate decisions made, mission success rate.
