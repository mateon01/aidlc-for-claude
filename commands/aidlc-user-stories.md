---
description: "AI-DLC: User Stories"
---

# AI-DLC: User Stories (INCEPTION Stage 4)

Create user stories with INVEST criteria through planning then generation.

## Execution

1. Load common rules from the `/aidlc` orchestrator command
2. CONDITIONAL: Assess whether user stories are needed (multi-factor)
3. Delegate to the `aidlc-story-writer` agent via Task tool (model: opus)
4. Pass context: aidlc-state.md, requirements.md, RE artifacts (if brownfield)
5. Two-part execution: Part 1 Planning (approval) → Part 2 Generation (approval)
6. Agent creates stories.md and personas.md in `aidlc-docs/inception/user-stories/`
7. Present approval gates (x2), record in audit.md, update aidlc-state.md

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-story-writer", model="opus",
     prompt="Create user stories for the project. [context]")
```

## Next Stage
- Workflow Planning
