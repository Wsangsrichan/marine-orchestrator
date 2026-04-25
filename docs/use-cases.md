# Real-World Use Cases

Concrete examples from bot-polymarket + haocomm-ai-oracle production work.

---

## Use Case 1: Multi-Agent Feature with Analytics Gate

**Scenario**: Implement take-profit at 50% PnL — but also verify statistically valid before touching thresholds.

```bash
/marine-orchestrator implement take-profit 50% with analytics verification
```

**Orchestrator plan:**
```
Sequential (analyst must gate trading):

marine-analyst  → check decisions.jsonl: n≥50 resolved trades?
                  → if n<50: STOP, "cannot conclude" hard rule
                  → if n≥50: proceed

marine-trading  → implement PnL gate in lotteryPositions.js
                  → add take-profit check in positionManager.js

⚠️ CRITICAL DECISION GATE → approve deploy

marine-devops   → ssh prod, git pull, pm2 restart
```

**State tracked:**
```json
{
  "mission": "take-profit-50",
  "github_issue": "#10",
  "agents": {
    "marine-analyst": "done",
    "marine-trading": "in_progress",
    "marine-devops": "pending"
  }
}
```

---

## Use Case 2: Multi-Repo Feature (Worktree)

**Scenario**: New AI provider needs backend change (bot-polymarket) + dashboard indicator (haocomm-ai-oracle) simultaneously.

```bash
/marine-orchestrator worktree feature-new-provider bot-polymarket haocomm-ai-oracle
```

**Orchestrator creates:**
```bash
git worktree add ../bot-polymarket-feature-new-provider feature/new-provider
git worktree add ../haocomm-feature-new-provider feature/new-provider
```

**Parallel assignment:**
```
marine-backend   → bot-polymarket worktree
  └─ add new AI provider integration in providers/

marine-frontend  → haocomm-ai-oracle worktree  
  └─ add provider status indicator in dashboard
```

**Orchestrator waits for both → coordinates merge → cleanup worktrees**

---

## Use Case 3: Production Deploy (Critical Gate)

```bash
/marine-orchestrator deploy bot-polymarket
```

**Always triggers critical gate:**
```
⚠️ CRITICAL DECISION: Deploy bot-polymarket to production
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What:  git pull + pm2 restart bot-polymarket on 204.168.182.194
Why:   take-profit fix + isResolved threshold fix ready
Risk:  ~10s downtime, affects live lottery positions (3 open)

Approve? (y/n):
```

After `y`:
```
marine-devops:
  ssh root@204.168.182.194
  cd /root/bot-polymarket
  git pull
  pm2 restart bot-polymarket
  pm2 logs --lines 20
```

---

## Use Case 4: Sprint Planning

```bash
/marine-orchestrator sprint
```

**Output:**
```
## Sprint Plan — 2026-04-25

### Open Issues

| # | Title | Size | Agent | Done When |
|---|-------|------|-------|-----------|
| #10 | Take-profit at 50% | M | marine-trading | positions close at 50% PnL |
| #11 | node_modules cleanup | S | marine-devops | 3GB recovered, confirmed each repo |
| #6 | blog-tomato posts | M | marine-content | 3+ posts published, SEO meta present |
| #7 | blog-tomato theme | L | marine-frontend | matches ipptt.com dark theme |

### Checkpoints
- Human approval: before any prod deploy
- marine-analyst gate: before #10 if threshold changes needed
- User confirmation: before each repo in #11

### Assignment Order
1. #10 (highest value — live capital impact)
2. #11 (quick win — disk space)
3. #6 + #7 (parallel — independent repos)
```

---

## Use Case 5: Daily Standup

```bash
/marine-orchestrator standup
```

**Output:**
```
## Daily Standup — 2026-04-25

✅ Done:
- Fixed isResolved threshold (lottery positions now visible)
- Deployed fix to prod (3 positions showing: Arsenal, Man City, Bengaluru FC)
- Phase 0.5 Code Recon added to marine-pilot
- marine-orchestrator skill created

🔄 Today:
- Issue #10: Take-profit at 50%
- Close Issue #9 (Bengaluru — not a bug)

🚧 Blockers:
- n=22 resolved trades — too small for strategy analysis (need 50+)
- journalctl vacuum needs interactive sudo (user runs manually)

📊 Bot status:
- PnL: -$60.51 | Win rate: 36.4% (n=22) | Open: 3 positions
- Note: n<50 — cannot draw strategy conclusions
```

---

## Use Case 6: Security Review Before Feature Ship

```bash
/marine-orchestrator security-check take-profit implementation
```

**Sequential:**
```
marine-security  → OWASP Top 10 scan on new code
                   check: no secrets in diffs
                   check: input validation on PnL threshold

marine-research  → architecture consistency review
                   does take-profit fit existing stop-loss pattern?

→ both approve → hand to marine-devops for deploy
```

---

## When NOT to Use Orchestrator

```bash
# Too small — just use marine-pilot
/marine-pilot fix typo in README
/marine-pilot update .env.example

# Single-agent task — marine-pilot handles it
/marine-pilot add loading spinner to dashboard

# Just exploring — use marine-research directly
"can you check how the analyzer.js processes confidence scores?"
```

**Rule of thumb**: If it needs 1 agent → `marine-pilot`. If it needs 2+ or a human gate → `marine-orchestrator`.
