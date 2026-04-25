# Worktree Workflow

Git worktrees let multiple agents work in **true isolation** — separate working directories, same repo, different branches. Essential for multi-repo features.

---

## When to Use

- Breaking change needs testing before touching prod branch
- Two repos need simultaneous coordinated changes
- Hotfix while main feature dev continues
- Risky experiment that shouldn't pollute main

---

## Creating Worktrees

```bash
/marine-orchestrator worktree feature-X bot-polymarket haocomm-ai-oracle
```

**Orchestrator runs:**
```bash
# In bot-polymarket
git worktree add ../bot-polymarket-feature-X feature/X

# In haocomm-ai-oracle
git worktree add ../haocomm-feature-X feature/X
```

**Then assigns agents:**
```
marine-backend   → ../bot-polymarket-feature-X/
marine-frontend  → ../haocomm-feature-X/
```

Both agents work in parallel. Orchestrator tracks progress in each worktree.

---

## Worktree Lifecycle

```
Create → Assign → Develop → PR → Review → [Human Gate] → Merge → Cleanup
```

### Cleanup

```bash
git worktree remove ../bot-polymarket-feature-X
git worktree remove ../haocomm-feature-X
```

Orchestrator handles cleanup automatically after merge approval.

---

## State Tracking

```json
{
  "mission": "feature-new-ai-provider",
  "worktrees": [
    {
      "repo": "bot-polymarket",
      "path": "../bot-polymarket-feature-new-provider",
      "branch": "feature/new-provider",
      "agent": "marine-backend",
      "status": "in_progress"
    },
    {
      "repo": "haocomm-ai-oracle",
      "path": "../haocomm-feature-new-provider",
      "branch": "feature/new-provider",
      "agent": "marine-frontend",
      "status": "pending"
    }
  ]
}
```

---

## vs Branches Only

| | Branches | Worktrees |
|--|----------|-----------|
| **Isolation** | Switch context | True separate dir |
| **Parallel agents** | One at a time | Simultaneous |
| **Multi-repo** | Manual | Coordinated |
| **Stash needed** | Yes | No |
| **Best for** | Sequential work | Parallel multi-repo |

Worktrees = branches without context switching. Each agent stays in its own working directory.
