---
title: Home
layout: home
nav_order: 1
---

# AI-DLC for Claude Code
{: .fs-9 }

A structured, adaptive software development workflow that guides AI through disciplined three-phase development.
{: .fs-6 .fw-300 }

[Get Started](/aidlc-for-claude/getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View on GitHub](https://github.com/mateon01/aidlc-for-claude){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What is AI-DLC?

**AI-DLC (AI-Driven Development Life Cycle)** is a methodology that brings structured software engineering discipline to AI-assisted development. Instead of ad-hoc coding, AI-DLC guides the AI through a three-phase lifecycle with human approval gates at every stage.

This plugin adapts the [AI-DLC methodology by AWS Labs](https://github.com/awslabs/aidlc-workflows) for **Claude Code**, leveraging its native command/agent architecture with multi-agent delegation and model tiering.

## Key Features

- **Adaptive Depth** -- Simple bug fixes skip unnecessary stages. Complex systems get full architectural treatment.
- **Three-Phase Lifecycle** -- INCEPTION (what and why), CONSTRUCTION (how), OPERATIONS (deploy and monitor).
- **Human-in-the-Loop** -- Every stage requires your explicit approval before proceeding.
- **Multi-Agent Delegation** -- Opus handles strategic reasoning, Sonnet handles volume work.
- **Full Audit Trail** -- Every decision documented with ISO 8601 timestamps.
- **Session Continuity** -- Interrupted workflows can be resumed from where you left off.
- **Brownfield Support** -- Deep reverse engineering for existing codebases with 8 analysis artifacts.

## The Three Phases

```
+-----------------------------------------------------------------------+
|                     INCEPTION (WHAT and WHY)                          |
|                                                                       |
|  Workspace Detection --> Reverse Engineering --> Requirements Analysis |
|  --> User Stories --> Workflow Planning --> Application Design         |
|  --> Units Generation                                                 |
+-----------------------------------+-----------------------------------+
                                    |
                                    v
+-----------------------------------------------------------------------+
|                     CONSTRUCTION (HOW)                                |
|                                                                       |
|  FOR each unit:                                                       |
|    Functional Design --> NFR Requirements --> NFR Design               |
|    --> Infrastructure Design --> Code Generation                      |
|                                                                       |
|  Build and Test (after all units)                                     |
+-----------------------------------+-----------------------------------+
                                    |
                                    v
+-----------------------------------------------------------------------+
|                     OPERATIONS (future)                               |
+-----------------------------------------------------------------------+
```

## Adaptive Execution

Not every stage runs every time. The workflow adapts to your needs:

| Condition | What Happens |
|:----------|:-------------|
| Simple bug fix | Only essential stages: detection, requirements, planning, code gen, test |
| New feature (greenfield) | Full INCEPTION + CONSTRUCTION treatment |
| Brownfield modification | Adds reverse engineering, adapts scope to existing codebase |
| Infrastructure-only change | Skips user stories and functional design |

## References

- [AI-DLC Methodology Blog (AWS Tech Blog)](https://aws.amazon.com/ko/blogs/tech/ai-driven-development-life-cycle/)
- [AI-DLC Interactive Demo](https://prod.d13rzhkk8cj2z0.amplifyapp.com/)
- [AI-DLC Workflows for Kiro (AWS Labs GitHub)](https://github.com/awslabs/aidlc-workflows)
