---
description: "AI-DLC: Units Generation"
---

# AI-DLC: Units Generation (INCEPTION Stage 7)

Decompose system into manageable units of work.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Execute if system needs decomposition into multiple units
3. Delegate to the `aidlc-units-planner` agent via Task tool (model: opus)
4. Pass context: aidlc-state.md, application-design artifacts, requirements.md, stories.md
5. Two-part execution: Part 1 Planning (approval) → Part 2 Generation (approval)
6. Agent creates unit artifacts in `aidlc-docs/inception/application-design/`
7. Present approval gates (x2), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-units-planner", model="opus",
     prompt="Decompose the system into units of work. Context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/application-design/, aidlc-docs/inception/requirements/")
```

## Next Stage
- CONSTRUCTION Phase (per-unit loop)
