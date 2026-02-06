---
description: "AI-DLC: Build and Test"
---

# AI-DLC: Build and Test (CONSTRUCTION Stage 6)

Generate comprehensive build and test instructions after all units complete.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. This stage ALWAYS executes once after all units complete code generation
3. Delegate to the `aidlc-build-test-engineer` agent via Task tool (model: sonnet)
4. Pass context: aidlc-state.md, all unit code summaries, all design artifacts
5. Agent creates instructions in `aidlc-docs/construction/build-and-test/`
6. Present approval gate, record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-build-test-engineer", model="sonnet",
     prompt="Generate build and test instructions for all completed units. [context]")
```

## Next Stage
- Operations (placeholder)
