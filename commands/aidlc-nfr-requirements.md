---
description: "AI-DLC: NFR Requirements"
---

# AI-DLC: NFR Requirements (CONSTRUCTION Stage 2)

Determine non-functional requirements and tech stack per unit.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Execute if performance/security/scalability concerns exist
3. Delegate to the `aidlc-nfr-analyst` agent via Task tool (model: sonnet)
4. Pass context: aidlc-state.md, unit definition, functional-design artifacts, requirements.md
5. Agent creates artifacts in `aidlc-docs/construction/{unit-name}/nfr-requirements/`
6. Present approval gate (2-option format), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-nfr-analyst", model="sonnet",
     prompt="Determine NFR requirements for unit [unit-name]. [context]")
```

## Next Stage
- NFR Design (conditional, requires this stage) or Infrastructure Design or Code Generation
