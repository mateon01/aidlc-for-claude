---
description: "AI-DLC: Workflow Planning"
---

# AI-DLC: Workflow Planning (INCEPTION Stage 5)

Determine which stages to execute and create the execution plan.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. This stage ALWAYS executes
3. Delegate to the `aidlc-workflow-planner` agent via Task tool (model: opus)
4. Pass context: aidlc-state.md, all prior artifacts (requirements, stories, RE)
5. Agent creates execution-plan.md in `aidlc-docs/inception/plans/`
6. Present approval gate, record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-workflow-planner", model="opus",
     prompt="Create workflow execution plan based on all prior context. Context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/requirements/, aidlc-docs/inception/user-stories/ (if exists), aidlc-docs/inception/reverse-engineering/ (if brownfield)")
```

## Next Stage
- Application Design (conditional) or Units Generation (conditional) or CONSTRUCTION
