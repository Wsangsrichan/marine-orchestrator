# marine-pilot vs marine-orchestrator

The two commands are **complementary, not competing**. Choose based on scope.

---

## At a Glance

| | `/marine-pilot` | `/marine-orchestrator` |
|--|----------------|----------------------|
| **Purpose** | Single task, full pipeline | Multi-agent coordination |
| **Agents** | 1 primary + QA reviewers | 2–5 agents in formation |
| **Repos** | 1 repo | 1 or many repos |
| **Worktrees** | No | Yes (parallel isolation) |
| **Ceremonies** | No | Sprint, standup, retro |
| **Human gates** | Optional (at plan approval) | Mandatory (critical actions) |
| **Best for** | Feature implementation | Multi-team coordination |

---

## marine-pilot — "One Marine, Full Mission"

`/marine-pilot` is the **autonomous executor**. Give it a task — it plans, implements, QA cycles, validates, and reports. One agent owns the full pipeline.

### Pipeline
```
Phase 0:   Understand (clarify if vague)
Phase 0.5: Code Recon (read actual code before planning)
Phase 1:   Plan (present for approval)
Phase 2:   Execute (model routing: Haiku/Sonnet/Opus)
Phase 3:   QA cycling (build → typecheck → lint → test)
Phase 4:   Validate (security + architecture review)
Phase 5:   Report
```

### Use When
```bash
/marine-pilot implement take-profit 50% for lottery positions
/marine-pilot fix the isResolved threshold bug in api.js
/marine-pilot add PWA support to the dashboard
```

### Characteristics
- **Single primary agent** doing the work
- **Parallel sub-agents** for QA and validation only
- **Code Recon** reads actual files before planning (Phase 0.5)
- Good for: bug fixes, features, refactors in one codebase

---

## marine-orchestrator — "Command Center, Multiple Units"

`/marine-orchestrator` is the **coordinator**. It doesn't do the work itself — it assigns right agent to right task, enforces gates, tracks state, and runs ceremonies.

### Pipeline
```
Phase 0: Parse intent (task type, repos, risk, agents)
Phase 1: Route agents (sequential or parallel)
Phase 2: Human gate (if critical)
Phase 3: Execute + track state
Phase 4: Report
```

### Use When
```bash
/marine-orchestrator implement take-profit + analytics verification
# → routes: marine-trading + marine-analyst + marine-devops

/marine-orchestrator sprint
# → lists open issues, estimates, assigns agents, defines done criteria

/marine-orchestrator deploy bot-polymarket
# → critical gate → marine-devops executes

/marine-orchestrator worktree feature-auth bot-polymarket haocomm-ai-oracle
# → parallel worktrees across 2 repos with agents assigned to each
```

### Characteristics
- **Multiple specialized agents** in formation
- **Parallel execution** when tasks are independent
- **Critical decision gate** before prod/capital actions
- **State tracking** in `.marine/orchestrator-state.json`
- Good for: multi-agent features, multi-repo, ceremonies, deploys

---

## Decision Tree

```
Task involves live capital / prod deploy?
  → YES: /marine-orchestrator deploy (mandatory human gate)
  → NO:  continue ↓

Task spans multiple repos?
  → YES: /marine-orchestrator worktree
  → NO:  continue ↓

Task needs 2+ specialized agents?
  → YES: /marine-orchestrator <task>
  → NO:  continue ↓

Single feature / bug fix / refactor?
  → /marine-pilot <task>
```

---

## Side-by-Side Example

**Scenario**: Implement take-profit at 50% PnL gain

### With marine-pilot
```
/marine-pilot implement take-profit 50% for lottery positions

Phase 0.5: Code Recon reads lotteryPositions.js + positionManager.js
Phase 1:   Plan → approve
Phase 2:   Execute → edit 2 files
Phase 3:   QA cycling
Phase 4:   Security + arch review
Phase 5:   Report
```
*Best when: just implementing the code change*

### With marine-orchestrator
```
/marine-orchestrator implement take-profit 50% with stats verification

Phase 0: Parse → need marine-trading + marine-analyst + marine-devops
Phase 1: Route:
  marine-trading  → implement PnL gate (lotteryPositions + positionManager)
  marine-analyst  → verify n≥50 trades before any threshold analysis
Phase 2: Human gate before deploy
Phase 3:
  marine-devops   → deploy to prod after approval
Phase 4: Report
```
*Best when: need verification + deploy coordination too*

---

## Combined Usage

They work together naturally:

```
/marine-orchestrator sprint
→ assigns Issue #10 to marine-pilot (take-profit)
→ assigns Issue #6 to marine-pilot (blog posts)
→ tracks both in parallel
→ coordinates final deploy via marine-devops
```

`marine-orchestrator` orchestrates. `marine-pilot` executes.
