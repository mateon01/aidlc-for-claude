---
description: "AI-DLC: Build and Test"
---

# AI-DLC: Build and Test (CONSTRUCTION Stage 6)

Build, test, and validate all completed units. Generates instruction documents AND executes actual builds and tests with up to 3 retry attempts on failure.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. This stage ALWAYS executes once after all units complete code generation
3. Delegate to the `aidlc-build-test-engineer` agent via Task tool (model: sonnet)
4. Pass context: aidlc-state.md, all unit code summaries, all design artifacts, workspace root path
5. Agent generates instruction documents in `aidlc-docs/construction/build-and-test/`
6. Agent detects build system, installs dependencies, builds, and runs tests via Bash
7. Agent retries failed steps up to 3 times with automated fix attempts
8. Agent creates execution report with actual results
9. Present approval gate, record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-build-test-engineer", model="sonnet",
     prompt="Build, test, and validate all completed units. Generate instruction documents then execute actual build and test commands. [context]")
```

## Next Stage
- Operations (placeholder)
