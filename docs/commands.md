# Commands & Agents

---

## Commands

All commands are prefixed with `/aidlc-for-claude:` when used in Claude Code.

### Main Orchestrator

| Command | Description |
|:--------|:------------|
| `/aidlc` | Entry point. Detects workspace, gathers requirements, executes full workflow |

### INCEPTION Phase

| Command | Stage | Description |
|:--------|:------|:------------|
| `/aidlc-workspace-detection` | 1 | Scan workspace, detect greenfield/brownfield |
| `/aidlc-reverse-engineering` | 2 | Analyze existing codebase (brownfield only) |
| `/aidlc-requirements-analysis` | 3 | Gather and analyze requirements |
| `/aidlc-user-stories` | 4 | Create user stories with INVEST criteria |
| `/aidlc-workflow-planning` | 5 | Determine execution plan |
| `/aidlc-application-design` | 6 | Component and service layer design |
| `/aidlc-units-generation` | 7 | Decompose system into implementation units |

### CONSTRUCTION Phase

| Command | Stage | Description |
|:--------|:------|:------------|
| `/aidlc-system-nfr` | 0 | System-level NFR decisions (multi-unit projects) |
| `/aidlc-functional-design` | 1 | Business logic design (per-unit) |
| `/aidlc-nfr-requirements` | 2 | Non-functional requirements (per-unit) |
| `/aidlc-nfr-design` | 3 | NFR pattern design (per-unit) |
| `/aidlc-infrastructure-design` | 4 | Infrastructure mapping (per-unit) |
| `/aidlc-code-generation` | 5 | Code + test generation with multi-layer quality gate (per-unit, two-part) |
| `/aidlc-build-and-test` | 6 | Build, test (with coverage), security scan, integration tests, and validate |

### OPERATIONS Phase

| Command | Description |
|:--------|:------------|
| `/aidlc-operations` | CI/CD pipeline, Dockerfile, .env.example, README, deployment checklist generation |

### Standalone Utilities

| Command | Description |
|:--------|:------------|
| `/aidlc-review-pr` | Analyze PR diffs for code quality, security, performance, and consistency |
| `/aidlc-ci-setup` | Generate CI/CD pipelines, PR review workflows, and issue/PR templates |
| `/aidlc-graph` | Build, update, visualize, export (PNG), search, verify, or tear down code dependency graphs (File/Neo4j/Neptune) with optional GraphRAG |

---

## Agents

Each command delegates to a specialized agent. Agents are tiered by model for cost-efficiency. Every agent displays a **stage banner (MOTD)** on start showing the current phase, stage number, agent name, model, and key capabilities.

!!! note "Agent Naming"
    Agents use the fully qualified `aidlc-for-claude:` prefix when invoked via the Task tool.

### :material-brain: Opus Agents (Strategic Reasoning)

| Agent | Purpose |
|:------|:--------|
| `aidlc-for-claude:aidlc-reverse-engineer` | Deep codebase analysis for brownfield projects |
| `aidlc-for-claude:aidlc-requirements-analyst` | Requirements gathering with 12-category deep questioning (15-25 questions, multi-round) |
| `aidlc-for-claude:aidlc-story-writer` | User story creation with 12-category analysis (error scenarios, edge cases, accessibility, privacy) |
| `aidlc-for-claude:aidlc-workflow-planner` | Execution planning and stage determination |
| `aidlc-for-claude:aidlc-application-designer` | Component and service design with 10-category architecture analysis |
| `aidlc-for-claude:aidlc-units-planner` | System decomposition with parallel group assignment and dependency analysis |
| `aidlc-for-claude:aidlc-code-planner` | Code generation plan with mandatory test plans per module |

### :material-lightning-bolt: Sonnet Agents (Volume Work)

| Agent | Purpose |
|:------|:--------|
| `aidlc-for-claude:aidlc-functional-designer` | Business logic and domain model design |
| `aidlc-for-claude:aidlc-system-nfr-analyst` | System-level NFR decisions for multi-unit projects |
| `aidlc-for-claude:aidlc-nfr-analyst` | Non-functional requirements assessment |
| `aidlc-for-claude:aidlc-nfr-designer` | NFR pattern and component design |
| `aidlc-for-claude:aidlc-infra-designer` | Infrastructure service mapping |
| `aidlc-for-claude:aidlc-code-generator` | Code + test generation with lint, type check, and design conformance |
| `aidlc-for-claude:aidlc-build-test-engineer` | Build, test (coverage), security scan, integration/E2E test scaffolding |
| `aidlc-for-claude:aidlc-ops-generator` | CI/CD, Dockerfile, Docker Compose, .env.example, README, deployment checklist |
| `aidlc-for-claude:aidlc-pr-reviewer` | PR diff analysis for code quality, security, performance, and consistency |
| `aidlc-for-claude:aidlc-ci-setup-engineer` | CI/CD pipeline, PR review workflow, and issue/PR template generation |
| `aidlc-for-claude:aidlc-graph-analyzer` | Code dependency graph with multi-backend support (File/Neo4j/Neptune), 9-point verification, impact analysis, Mermaid visualization, PNG export, and GraphRAG summary-based retrieval |

### :material-speedometer: Haiku Agents (Fast Detection)

| Agent | Purpose |
|:------|:--------|
| `aidlc-for-claude:aidlc-workspace-analyst` | Workspace scanning and project type detection |

---

## Two-Part Stages

Three stages use a two-part approach for quality assurance:

| Stage | Part 1 (Plan) | Part 2 (Execute) |
|:------|:--------------|:-----------------|
| User Stories | Story plan with personas | Individual story generation |
| Units Generation | Decomposition plan | Unit definition documents |
| Code Generation | File-by-file code plan (Opus) | Actual code generation (Sonnet) |

!!! note
    In each case, you approve the plan before execution begins.
