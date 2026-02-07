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

{: .note }
> AI-DLC was originally developed by [AWS Labs](https://github.com/awslabs/aidlc-workflows) for Kiro. This plugin adapts the methodology for **Claude Code** with multi-agent delegation and model tiering.

---

## What is AI-DLC?

**AI-DLC (AI-Driven Development Life Cycle)** is a methodology that brings structured software engineering discipline to AI-assisted development. Instead of ad-hoc coding, AI-DLC guides the AI through a three-phase lifecycle with human approval gates at every stage.

### Why AI-DLC?

| Without AI-DLC | With AI-DLC |
|:----------------|:------------|
| AI jumps straight to coding | Structured analysis before implementation |
| No documentation trail | Full audit trail with ISO 8601 timestamps |
| One-size-fits-all approach | Adaptive depth based on complexity |
| Single model for everything | Opus for strategy, Sonnet for execution |
| Hard to resume interrupted work | Session continuity via state tracking |
| No approval gates | Human-in-the-loop at every stage |

---

## Key Features

{: .tip }
> Just run `/aidlc` and the workflow adapts to your project automatically.

**Adaptive Depth**
: Simple bug fixes skip unnecessary stages. Complex systems get full architectural treatment.

**Three-Phase Lifecycle**
: INCEPTION (what and why) &#8594; CONSTRUCTION (how) &#8594; OPERATIONS (deploy and monitor).

**Human-in-the-Loop**
: Every stage requires your explicit approval before proceeding.

**Multi-Agent Delegation**
: Opus handles strategic reasoning, Sonnet handles volume work.

**Full Audit Trail**
: Every decision documented with ISO 8601 timestamps.

**Session Continuity**
: Interrupted workflows can be resumed from where you left off.

**Brownfield Support**
: Deep reverse engineering for existing codebases with 8 analysis artifacts.

---

## The Three Phases

### INCEPTION -- What and Why
{: .text-purple-300 }

Understands the problem space. Detects workspace type, gathers requirements, creates user stories, plans the execution, designs architecture, and decomposes into implementation units.

> 7 stages: Workspace Detection &#8594; Reverse Engineering &#8594; Requirements Analysis &#8594; User Stories &#8594; Workflow Planning &#8594; Application Design &#8594; Units Generation

### CONSTRUCTION -- How
{: .text-green-300 }

Implements the system. For each unit: designs business logic, evaluates non-functional requirements, maps infrastructure, generates code. Finishes with build and test.

> 6 stages (per-unit loop): Functional Design &#8594; NFR Requirements &#8594; NFR Design &#8594; Infrastructure Design &#8594; Code Generation &#8594; Build & Test

### OPERATIONS -- Future
{: .text-yellow-300 }

Deployment, monitoring, and operational concerns. Coming in a future release.

---

## Adaptive Execution

Not every stage runs every time. The workflow adapts to your needs:

| Condition | What Happens |
|:----------|:-------------|
| **Simple bug fix** | Only essential stages: detection, requirements, planning, code gen, test |
| **New feature (greenfield)** | Full INCEPTION + CONSTRUCTION treatment |
| **Brownfield modification** | Adds reverse engineering, adapts scope to existing codebase |
| **Infrastructure-only change** | Skips user stories and functional design |

{: .note }
> You can override any recommendation at the Workflow Planning approval gate.

---

## References

- [AI-DLC Methodology Blog (AWS Tech Blog)](https://aws.amazon.com/ko/blogs/tech/ai-driven-development-life-cycle/) -- Original methodology introduction
- [AI-DLC Interactive Demo](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) -- Live demo of the AI-DLC workflow
- [AI-DLC Workflows for Kiro (AWS Labs GitHub)](https://github.com/awslabs/aidlc-workflows) -- Original Kiro steering files (377+ stars)
