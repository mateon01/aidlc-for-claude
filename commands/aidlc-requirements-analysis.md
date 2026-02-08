---
description: "AI-DLC: Requirements Analysis"
---

# AI-DLC: Requirements Analysis (INCEPTION Stage 3)

Gather and analyze requirements with adaptive depth.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. Delegate to the `aidlc-requirements-analyst` agent via Task tool (model: opus)
3. Pass context: aidlc-state.md, RE artifacts (if brownfield), user's original request
4. Agent creates requirements.md and question files in `aidlc-docs/inception/requirements/`
5. Present approval gate, record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-requirements-analyst", model="opus",
     prompt="Analyze requirements for the project. Context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/reverse-engineering/ (if brownfield)")
```

## Next Stage
- User Stories (conditional) or Workflow Planning
