---
description: "AI-DLC: Reverse Engineering"
---

# AI-DLC: Reverse Engineering (INCEPTION Stage 2)

Deep analysis of existing codebase for brownfield projects.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. Verify: workspace must be brownfield with no existing RE artifacts
3. Delegate to the `aidlc-reverse-engineer` agent via Task tool (model: opus)
4. Pass context: aidlc-state.md, workspace root path
5. Agent generates 8 artifacts in `aidlc-docs/inception/reverse-engineering/`
6. Present approval gate, record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-reverse-engineer", model="opus",
     prompt="Analyze the existing codebase and generate reverse engineering artifacts. [context]")
```

## Next Stage
- Requirements Analysis
