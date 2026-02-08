---
description: "AI-DLC: Functional Design"
---

# AI-DLC: Functional Design (CONSTRUCTION Stage 1)

Detailed business logic design per unit (technology-agnostic).

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Execute if new data models or complex logic needed
3. Delegate to the `aidlc-functional-designer` agent via Task tool (model: sonnet)
4. Pass context: aidlc-state.md, unit definition, assigned stories, application-design artifacts
5. Agent creates artifacts in `aidlc-docs/construction/{unit-name}/functional-design/`
6. Present approval gate (2-option format), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-functional-designer", model="sonnet",
     prompt="Create functional design for unit [unit-name]. Context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/application-design/, aidlc-docs/inception/requirements/, aidlc-docs/construction/{prior-unit}/functional-design/ (if unit N > 1)")
```

## Next Stage
- NFR Requirements (conditional) or NFR Design or Infrastructure Design or Code Generation
