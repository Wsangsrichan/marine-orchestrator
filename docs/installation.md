# Installation

ติดตั้ง marine-pilot และ marine-orchestrator สำหรับ Claude Code

---

## Prerequisites

- [Claude Code](https://claude.ai/code) (CLI หรือ IDE extension)
- Node.js 18+
- Git

---

## โครงสร้าง ~/.claude/

```
~/.claude/
├── skills/
│   ├── marine-pilot/
│   │   └── SKILL.md          ← /marine-pilot command
│   └── marine-orchestrator/
│       └── SKILL.md          ← /marine-orchestrator command
└── agents/
    └── marine/               ← sub-agents ที่ pilot/orchestrator spawn
        ├── marine-backend.md
        ├── marine-frontend.md
        ├── marine-research.md
        ├── marine-trading.md
        ├── marine-devops.md
        ├── marine-security.md
        ├── marine-analyst.md
        ├── marine-content.md
        ├── marine-ai.md
        ├── marine-prompt-engineer.md
        └── marine-prime.md
```

---

## ติดตั้ง marine-pilot

```bash
mkdir -p ~/.claude/skills/marine-pilot
curl -o ~/.claude/skills/marine-pilot/SKILL.md \
  https://raw.githubusercontent.com/Wsangsrichan/gsd-instruction/main/templates/marine-pilot/SKILL.md
```

ทดสอบใน Claude Code:
```
/marine-pilot
```

---

## ติดตั้ง marine-orchestrator

```bash
mkdir -p ~/.claude/skills/marine-orchestrator
curl -o ~/.claude/skills/marine-orchestrator/SKILL.md \
  https://raw.githubusercontent.com/Wsangsrichan/gsd-instruction/main/templates/marine-orchestrator/SKILL.md
```

ทดสอบ:
```
/marine-orchestrator sprint
```

---

## ติดตั้ง Marine Agents (ทั้งหมด)

agents คือ sub-agents ที่ถูก spawn โดย marine-pilot และ marine-orchestrator

```bash
mkdir -p ~/.claude/agents/marine
for agent in marine-backend marine-frontend marine-research marine-trading \
             marine-devops marine-security marine-analyst marine-content \
             marine-ai marine-prompt-engineer marine-prime; do
  curl -o ~/.claude/agents/marine/${agent}.md \
    https://raw.githubusercontent.com/Wsangsrichan/gsd-instruction/main/templates/agents/${agent}.md
done
```

---

## Marine Agents

| Agent | บทบาท |
|-------|--------|
| `marine-backend` | API, database, server-side logic |
| `marine-frontend` | UI components, CSS, UX |
| `marine-research` | Code exploration, tech comparison |
| `marine-trading` | Order lifecycle, PnL, exchanges |
| `marine-devops` | SSH, deploy, pm2, Docker |
| `marine-security` | OWASP review, threat model |
| `marine-analyst` | Statistics, PnL analysis (n-threshold enforcement) |
| `marine-content` | Blog posts, docs, markdown |
| `marine-ai` | Architecture, AI/LLM decisions |
| `marine-prompt-engineer` | Prompt design, token optimization |
| `marine-prime` | Lead orchestration, complex coordination |

---

## SKILL.md Format

skill file บอก Claude Code ว่า command ทำงานอย่างไร — ต้องมี YAML frontmatter

```markdown
---
name: marine-orchestrator
description: Multi-agent orchestration — coordinate marine team across repos, worktrees, sprints, and critical decision gates
---

# /marine-orchestrator

> "All units, move in formation."

## Usage

```bash
/marine-orchestrator <task>
/marine-orchestrator sprint
/marine-orchestrator standup
/marine-orchestrator deploy <repo>
/marine-orchestrator worktree <feature> <repos>
```

## Pipeline

```
Phase 0: Parse intent
Phase 1: Route to agents (sequential or parallel)
Phase 2: Human gate (if critical)
Phase 3: Execute + track state
Phase 4: Report
```
```

---

## Agent Format

agent file คือ prompt template สำหรับ sub-agent

```markdown
# Marine Backend Engineer

Role: Builds and maintains APIs, databases, integrations, and server-side logic.

## Expertise
- API design (REST, GraphQL, WebSocket)
- Database modeling, migrations, query optimization
- Node.js, Python, Go

## When to Use
- Work involves server-side code, API contracts, schema changes
- Database operations or query optimization

## Workflow
1. Read existing code and understand current patterns
2. Propose changes with clear rationale
3. Implement with proper error handling
4. Verify with tests or manual checks

## Quality Standards
- All API endpoints must have proper error responses
- Database queries must be parameterized (no SQL injection)
- Sensitive data must not be logged

## RTK — Token Optimization (MANDATORY)
Always prefix bash: `rtk git status`, `rtk ls`, `rtk read <file>`
Skip rtk only for: npm install, npx, gh, git push
```

---

## สร้าง Custom Agent

```bash
cat > ~/.claude/agents/marine/marine-ml.md << 'EOF'
# Marine ML Engineer

Role: Builds and maintains machine learning pipelines and inference.

## Expertise
- Model training, evaluation, deployment
- Feature engineering, data preprocessing
- Python: PyTorch, scikit-learn, pandas

## When to Use
- Implementing ML models or pipelines
- Model evaluation and A/B testing

## Workflow
1. Understand the ML problem (classification/regression)
2. Review existing data pipeline
3. Implement with reproducibility (fixed seeds, versioned artifacts)
4. Evaluate with proper train/val/test split

## Quality Standards
- Never train on test data
- Always log model metrics
- Reproducible experiments

## RTK — Token Optimization (MANDATORY)
Always prefix bash: rtk git status, rtk ls, rtk read
EOF
```

---

## Verify

```bash
ls ~/.claude/skills/
# marine-pilot/  marine-orchestrator/

ls ~/.claude/agents/marine/
# marine-ai.md  marine-analyst.md  marine-backend.md ...
```

ใน Claude Code พิมพ์ `/marine-orchestrator` → เห็น usage = ติดตั้งสำเร็จ
