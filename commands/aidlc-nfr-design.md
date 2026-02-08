---
description: "AI-DLC: NFR Design"
---

# AI-DLC: NFR Design (CONSTRUCTION Stage 3)

Incorporate NFR requirements into design using patterns.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Execute only if NFR Requirements stage was executed
3. Delegate to the `aidlc-nfr-designer` agent via Task tool (model: sonnet)
4. Pass context: aidlc-state.md, unit definition, nfr-requirements artifacts, functional-design artifacts
5. Agent creates artifacts in `aidlc-docs/construction/{unit-name}/nfr-design/`
6. Present approval gate (2-option format), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-nfr-designer", model="sonnet",
     prompt="Design NFR patterns for unit [unit-name]. Context: aidlc-docs/aidlc-state.md, aidlc-docs/construction/{unit-name}/nfr-requirements/")
```

## Next Stage
- Infrastructure Design (conditional) or Code Generation
