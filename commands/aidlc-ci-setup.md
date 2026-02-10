---
description: "AI-DLC: CI Setup - Generate CI/CD pipelines, PR review workflows, and issue/PR templates for any project"
---

# AI-DLC: CI Setup (Standalone Utility)

Generate CI/CD infrastructure files tailored to your project's tech stack. Detects language, build system, test framework, and linter automatically.

## Execution

1. This is a standalone utility command -- it can be run independently of the AI-DLC three-phase workflow
2. No prerequisite stages required
3. No approval gate (generates files at workspace root)

### CI Setup
4. Delegate to the `aidlc-ci-setup-engineer` agent via Task tool (model: sonnet)
5. Agent detects tech stack, asks what to generate, and creates selected files
6. Present summary with next steps

## Agent Delegation

```
Task(subagent_type="aidlc-for-claude:aidlc-ci-setup-engineer", model="sonnet",
     prompt="Detect project tech stack and generate selected CI/CD infrastructure files.")
```
