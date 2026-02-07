---
description: "AI-DLC: Code Generation"
---

# AI-DLC: Code Generation (CONSTRUCTION Stage 5)

Generate code from approved design artifacts. Two-part: Planning then Execution.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. This stage ALWAYS executes for each unit

### Part 1: Planning
3. Delegate to the `aidlc-code-planner` agent via Task tool (model: opus)
4. Pass context: aidlc-state.md, all design artifacts for the unit, RE artifacts (if brownfield)
5. Agent creates code plan in `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md`
6. Present approval gate for the plan

### Part 2: Execution
7. Delegate to the `aidlc-code-generator` agent via Task tool (model: sonnet)
8. Pass context: approved code plan, all design artifacts
9. Agent generates code at workspace root (NEVER in aidlc-docs/)
10. Present approval gate for generated code
11. Record in audit.md, update aidlc-state.md

## Agent Delegation

```
# Part 1
Task(subagent_type="aidlc-for-claude:aidlc-code-planner", model="opus",
     prompt="Create code generation plan for unit [unit-name]. [context]")

# Part 2
Task(subagent_type="aidlc-for-claude:aidlc-code-generator", model="sonnet",
     prompt="Execute the approved code plan for unit [unit-name]. [context]")
```

## Next Stage
- Next unit (if more units remain) or Build and Test (after all units)
