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
- :material-pipe: **CI Setup** -- Standalone utility to generate CI/CD pipelines, PR review workflows, and issue/PR templates with automatic tech stack detection.
- :material-graph-outline: **Dependency Graph** -- Optional graph-based code dependency analysis with multi-backend support (File/Neo4j/Neptune), impact analysis, Mermaid visualization, PNG export, 9-point deployment verification, CGIG compilation repair, and change-aware test prioritization. E2E verified with Neo4j and Neptune backends.
- :material-text-search: **GraphRAG** -- Optional summary-based semantic code retrieval. Claude generates module summaries and community structure stored as graph properties -- no external embedding models required. Search by purpose, find related modules, understand code semantics.
- :material-wrench-cog: **CGIG Repair** -- Compilation-Guided Iterative Graph-retrieval for automated error repair. 4 graph construction methods (Static/CGIG/Lightweight/Hybrid), 10 language-agnostic error categories, confidence-scored fix suggestions from dependency graph context.

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

### AI-DLC Methodology

- :material-newspaper: [AI-DLC Methodology Blog (AWS Tech Blog)](https://aws.amazon.com/ko/blogs/tech/ai-driven-development-life-cycle/) -- Original methodology introduction
- :material-play-circle: [AI-DLC Interactive Demo](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) -- Live demo of the AI-DLC workflow
- :material-github: [AI-DLC Workflows for Kiro (AWS Labs GitHub)](https://github.com/awslabs/aidlc-workflows) -- Original Kiro steering files

### Code Graph and CGIG Research

The CGIG feature (v1.8.0) is based on research from SELENE Lab, Korea University. These papers inform graph-based code generation and repair:

- :material-file-document: **CGIG: Compilation-Guided Iterative Graph-Retrieval for LLM-Based Brownfield Code Generation** -- SELENE Lab, Korea University. Reactive compile-parse-query-fix loop achieving 98% compilation success rate on Apache Lucene (1.2M LOC), up from 72% single-shot baseline.
- :material-graph-outline: Liu et al. (2024). [RepoGraph: Enhancing AI Software Engineering with Repository-level Code Graph](https://arxiv.org/abs/2410.14684). *arXiv:2410.14684*. -- Repository-level dependency graph for code completion.
- :material-graph-outline: Liu et al. (2024). [CodeXGraph: Bridging LLMs and Code Repositories via Code Graph Databases](https://arxiv.org/abs/2408.05978). *arXiv:2408.05978*. -- Graph DB interface for LLM agents to query repository structure.
- :material-graph-outline: Liu et al. (2024). [GraphCoder: Enhancing Repository-Level Code Completion via Code Context Graph-based Retrieval](https://arxiv.org/abs/2406.07003). *arXiv:2406.07003*. -- Code graph retrieval with generation integration.
- :material-bug: Chen et al. (2024). [Teaching Large Language Models to Self-Debug](https://openreview.net/forum?id=KuPixIqPiq). *ICLR 2024*. -- LLM self-debugging via execution feedback.
- :material-bug: Zhong et al. (2024). [LDB: A Large Language Model Debugger via Verifying Runtime Execution Step-by-step](https://aclanthology.org/2024.acl-long.866/). *ACL 2024*. -- Structured debugging with runtime traces.
