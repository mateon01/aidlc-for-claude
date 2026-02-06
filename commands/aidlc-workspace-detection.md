---
description: "AI-DLC: Workspace Detection"
---

# AI-DLC: Workspace Detection (INCEPTION Stage 1)

Scan the workspace to determine project type and existing state.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. Delegate to the `aidlc-workspace-analyst` agent via Task tool (model: sonnet)
3. Pass context: user's original request, workspace root path
4. This stage ALWAYS executes and auto-proceeds (no approval gate)
5. After completion, update `aidlc-docs/aidlc-state.md`

## Agent Delegation

```
Task(subagent_type="aidlc-workspace-analyst", model="sonnet",
     prompt="Scan the workspace and determine project state. [context]")
```

## Next Stage
- If brownfield (existing code): Reverse Engineering
- If greenfield (empty): Requirements Analysis
