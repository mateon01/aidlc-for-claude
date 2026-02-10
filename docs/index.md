# AI-DLC for Claude Code

**A structured, adaptive software development workflow that guides AI through disciplined three-phase development.**

:material-rocket-launch: [**Get Started**](getting-started.md) -- Just run `/aidlc` and the workflow adapts automatically.
{ .md-button }

:material-github: [**View on GitHub**](https://github.com/mateon01/aidlc-for-claude) -- Apache-2.0 licensed. Adapted from AWS Labs AI-DLC.
{ .md-button }

---

!!! info "Origin"
    AI-DLC was originally developed by [AWS Labs](https://github.com/awslabs/aidlc-workflows) for Kiro. This plugin adapts the methodology for **Claude Code** with multi-agent delegation and model tiering.

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

!!! tip "One Command"
    Just run `/aidlc` and the workflow adapts to your project automatically.

- :material-auto-fix: **Adaptive Depth** -- Simple bug fixes skip unnecessary stages. Complex systems get full architectural treatment.
- :material-chat-question: **Deep Questioning** -- INCEPTION stages ask 15-30 practical questions across mandatory categories with multi-round follow-up. No more shallow Q&A.
- :material-transit-connection-variant: **Three-Phase Lifecycle** -- INCEPTION (what and why) &rarr; CONSTRUCTION (how) &rarr; OPERATIONS (deploy and monitor).
- :material-account-check: **Human-in-the-Loop** -- Every stage requires your explicit approval before proceeding.
- :material-robot: **Multi-Agent Delegation** -- Opus handles strategic reasoning, Sonnet handles volume work, Haiku handles fast detection.
- :material-run-fast: **Parallel Unit Execution** -- For multi-unit projects (3+), independent units execute simultaneously in parallel groups, reducing total build time.
- :material-file-document-check: **Full Audit Trail** -- Every decision documented with ISO 8601 timestamps.
- :material-format-header-pound: **Stage Banners (MOTD)** -- Every agent displays a formatted banner on start showing phase, stage number, agent name, model, and capabilities.
- :material-restart: **Session Continuity** -- Interrupted workflows can be resumed from where you left off.
- :material-magnify-scan: **Brownfield Support** -- Deep reverse engineering for existing codebases with 8 analysis artifacts.
- :material-source-pull: **PR Review** -- Standalone utility for analyzing PR diffs across 6 categories (correctness, security, performance, consistency, testing, documentation).

---

## The Three Phases

### :material-lightbulb-on:{ .lg } INCEPTION -- What and Why

Understands the problem space. Detects workspace type, gathers requirements, creates user stories, plans the execution, designs architecture, and decomposes into implementation units.

```
Workspace Detection → Reverse Engineering → Requirements Analysis
→ User Stories → Workflow Planning → Application Design → Units Generation
```

### :material-hammer-wrench:{ .lg } CONSTRUCTION -- How

Implements the system. For each unit (sequentially or in parallel): designs business logic, evaluates non-functional requirements, maps infrastructure, generates code. Finishes with build and test.

```
Per-Unit Loop (sequential or parallel):
  Functional Design → NFR Requirements → NFR Design
  → Infrastructure Design → Code Generation

Build and Test (after all units)
```

### :material-server:{ .lg } OPERATIONS -- Future

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

!!! note
    You can override any recommendation at the Workflow Planning approval gate.

---

## References

- :material-newspaper: [AI-DLC Methodology Blog (AWS Tech Blog)](https://aws.amazon.com/ko/blogs/tech/ai-driven-development-life-cycle/) -- Original methodology introduction
- :material-play-circle: [AI-DLC Interactive Demo](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) -- Live demo of the AI-DLC workflow
- :material-github: [AI-DLC Workflows for Kiro (AWS Labs GitHub)](https://github.com/awslabs/aidlc-workflows) -- Original Kiro steering files
