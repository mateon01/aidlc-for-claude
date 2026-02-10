---
description: "AI-DLC: Review PR - Analyze pull request diffs for code quality, security, and consistency"
---

# AI-DLC: PR Review (Standalone Utility)

Analyze pull request diffs or local changes for code quality, security, performance, and consistency.

## Execution

1. This is a standalone utility command -- it can be run independently of the AI-DLC three-phase workflow
2. No prerequisite stages required
3. No approval gate (read-only analysis)

### PR Review
4. Delegate to the `aidlc-pr-reviewer` agent via Task tool (model: sonnet)
5. Agent performs 6-category analysis: correctness, security, performance, consistency, testing, documentation
6. Present structured review report

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-pr-reviewer", model="sonnet",
     prompt="Analyze PR or local changes for code quality, security, performance, and consistency.")
```
