# Quality Gates

Non-negotiable rules that orchestrator enforces regardless of instruction.

---

## Gate 1: Code Recon Before Planning

Inherited from marine-pilot Phase 0.5.

```
Before any implementation plan:
→ Explore agent reads actual code
→ Finds real root cause (not assumed)
→ Only then: plan
```

**Why**: Prevents "answering the wrong question." Real example: user said "Open Positions has no cost field" → recon found the real bug was `isResolved` threshold too wide, not missing field.

---

## Gate 2: marine-analyst n≥50 Rule

```
Task involves: confidence threshold, edge %, stop-loss %, strategy change
n < 50 resolved trades: → HARD STOP
  Report: "Cannot conclude. Need 50+ resolved trades (current: n=X)"
```

**Why**: With n=22, a 36% win rate could be noise. Wilson CI is too wide to draw conclusions. Any "improvement" based on small sample is p-hacking.

---

## Gate 3: Prompt Change Distribution Check

```
Before JSON prompt change:
  marine-analyst: check current action distribution
  
After change:
  marine-analyst: verify distribution looks healthy
  
If distribution shifts significantly:
  → Flag, do not deploy
```

**Why**: Prompt changes can silently shift action bias (all YES, all NO, skewed confidence). Before/after check catches this.

---

## Gate 4: Prod Deploy Human Gate

```
ALL prod deploys → mandatory ⚠️ CRITICAL DECISION format
No exceptions. No auto-approve.
```

**Why**: Live capital. Even a 10s restart affects open positions.

---

## Gate 5: Capital / Strategy = Human Decision

```
Orchestrator NEVER decides:
- Whether to increase position size
- Whether to change stop-loss %
- Whether to close positions manually
- Whether strategy changes are "worth it"

Orchestrator ALWAYS:
- Presents data
- Shows risk
- Asks for approval
```

**Principle**: External Brain, Not Command. AI helps think. Human decides.

---

## Gate Enforcement vs Override

Gates 1, 4, 5: **Cannot be overridden**
Gates 2, 3: Can be explicitly overridden with conscious acknowledgment

```
"I know n<50, I want to see the analysis anyway"
→ Orchestrator shows analysis but still labels: "CAUTION: n=22, insufficient for conclusions"
```
