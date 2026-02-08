---
description: "AI-DLC: Application Design"
---

# AI-DLC: Application Design (INCEPTION Stage 6)

High-level component identification and service layer design.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Execute if new components/services needed
3. Delegate to the `aidlc-application-designer` agent via Task tool (model: opus)
4. Pass context: aidlc-state.md, requirements.md, stories.md, execution-plan.md
5. Agent creates 4 artifacts in `aidlc-docs/inception/application-design/`
6. Present approval gate, record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-application-designer", model="opus",
     prompt="Design application components and service layers. Context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/requirements/, aidlc-docs/inception/user-stories/ (if exists)")
```

## Next Stage
- Units Generation (conditional) or CONSTRUCTION
