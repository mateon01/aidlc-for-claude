---
description: "AI-DLC: Operations"
---

# AI-DLC: Operations (OPERATIONS Phase)

Generate deployment artifacts and developer documentation from completed construction artifacts.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. This stage executes after Build and Test passes

### Deployment Checklist
3. Delegate to the `aidlc-ops-generator` agent via Task tool (model: sonnet)
4. Pass context: aidlc-docs/aidlc-state.md, aidlc-docs/construction/*/infrastructure-design/, aidlc-docs/construction/*/nfr-design/
5. Agent creates deployment checklist and developer README
6. Present approval gate

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-ops-generator", model="sonnet",
     prompt="Generate deployment checklist and developer README. Context: aidlc-docs/aidlc-state.md, aidlc-docs/construction/*/infrastructure-design/, aidlc-docs/construction/*/nfr-design/")
```
