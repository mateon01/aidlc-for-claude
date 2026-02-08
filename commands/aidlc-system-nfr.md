---
description: "AI-DLC: System NFR Assessment"
---

# AI-DLC: System NFR Assessment (CONSTRUCTION Stage 0)

Establish system-wide non-functional requirements and cross-cutting decisions before the per-unit loop.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. This stage executes ONCE before the per-unit construction loop (skip if only 1 unit)
3. Delegate to the `aidlc-system-nfr-analyst` agent via Task tool (model: sonnet)
4. Pass context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/application-design/, aidlc-docs/inception/requirements/
5. Agent creates system NFR decisions in `aidlc-docs/construction/system-nfr-decisions.md`
6. Present approval gate (2-option format), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-system-nfr-analyst", model="sonnet",
     prompt="Establish system-wide NFR decisions. Context: aidlc-docs/aidlc-state.md, aidlc-docs/inception/application-design/, aidlc-docs/inception/requirements/")
```

## Next Stage
- Per-unit Construction loop (Functional Design for first unit)
