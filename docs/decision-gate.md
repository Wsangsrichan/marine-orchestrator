# Critical Decision Gate

The most important safety mechanism in marine-orchestrator. **Never skipped. Never auto-approved.**

---

## What Triggers It

The gate is mandatory before any of these:

| Action | Why |
|--------|-----|
| Production deploy | Affects live users/positions |
| `positions.json` reset | Destroys trade history |
| Trade close (force) | Irreversible capital action |
| Strategy changes | confidence threshold, edge %, stop-loss % |
| Deleting files on prod | Data loss |
| Any action affecting live capital | Human must decide |

---

## Gate Format

```
⚠️ CRITICAL DECISION: [Action Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  [Specific action with details]
Why:   [Reasoning — what changed, why now]
Risk:  [Impact to prod/positions/capital]

Approve? (y/n):
```

### Real Examples

**Deploy gate:**
```
⚠️ CRITICAL DECISION: Deploy bot-polymarket to production
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  git pull + pm2 restart on 204.168.182.194:/root/bot-polymarket
Why:   isResolved threshold fix ready (cur < 0.005 + redeemable flag)
Risk:  ~10s downtime; 3 open positions (Arsenal, Man City, Bengaluru FC)

Approve? (y/n):
```

**Strategy change gate:**
```
⚠️ CRITICAL DECISION: Change confidence threshold
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  Raise MIN_CONFIDENCE from 0.72 to 0.80 in config.js
Why:   marine-analyst suggests higher threshold improves edge
Risk:  Will reject more trades. Affects live position sizing. n=22 (below 50 minimum)

⛔ BLOCKED: n=22 resolved trades < 50 minimum.
marine-analyst rule: cannot conclude strategy changes.

Approve? (y/n):
```
*(Even if you say yes, orchestrator will refuse until n≥50)*

---

## What Happens After Approval

`y` → orchestrator routes to `marine-devops` with full context:
- What was approved
- Exact commands to run
- Verification steps after

`n` → mission stops cleanly:
```
Decision rejected. Mission paused.
State saved: .marine/orchestrator-state.json
Resume with: /marine-orchestrator resume
```

---

## Principle

> Human decides on capital and strategy. Orchestrator never auto-decides.

This is Rule 3 from the Oracle principles: **"External Brain, Not Command"** — AI helps think, human decides.
