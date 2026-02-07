---
description: "AI-DLC: Infrastructure Design"
---

# AI-DLC: Infrastructure Design (CONSTRUCTION Stage 4)

Map logical components to actual infrastructure services.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Execute if infrastructure mapping is needed
3. Delegate to the `aidlc-infra-designer` agent via Task tool (model: sonnet)
4. Pass context: aidlc-state.md, unit definition, nfr-design artifacts, functional-design artifacts
5. Agent creates artifacts in `aidlc-docs/construction/{unit-name}/infrastructure-design/`
6. Present approval gate (2-option format), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-infra-designer", model="sonnet",
     prompt="Design infrastructure mapping for unit [unit-name]. [context]")
```

## Next Stage
- Code Generation
