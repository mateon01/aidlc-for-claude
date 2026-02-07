# AI-DLC for Claude Code

A Claude Code plugin that implements the **AI-DLC (AI-Driven Development Life Cycle)** methodology -- a structured, adaptive software development workflow that guides AI assistants through disciplined three-phase development.

AI-DLC intelligently adapts to your project: simple changes execute only essential stages, while complex projects receive comprehensive treatment with full safeguards.

**[Documentation](https://mateon01.github.io/aidlc-for-claude)** | **[Apache-2.0 License](LICENSE)**

## Installation

**Claude Code CLI:**

```bash
claude plugin marketplace add https://github.com/mateon01/aidlc-for-claude
claude plugin install aidlc-for-claude
```

## Quick Start

```
/aidlc
```

That's it. The orchestrator detects your workspace (greenfield or brownfield), gathers requirements through interactive Q&A, and walks through each phase with you. Every stage requires your approval before proceeding.

To run a specific stage independently:

```
/aidlc-requirements-analysis
/aidlc-code-generation
```

## How It Works

AI-DLC has three phases. Each phase contains stages that may execute conditionally based on your project's needs.

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

### Stage Execution

Not every stage runs every time. After Workflow Planning, an execution plan determines which stages are needed:

| Condition | Stages Executed |
|-----------|----------------|
| Simple bug fix | Workspace Detection, Requirements, Workflow Planning, Code Generation, Build and Test |
| New feature (greenfield) | All INCEPTION + all CONSTRUCTION stages |
| Brownfield modification | Adds Reverse Engineering, adapts scope based on existing codebase |
| Infrastructure-only change | Skips User Stories, Functional Design; includes Infrastructure Design |

You can override any recommendation at the Workflow Planning approval gate.

## Commands

| Command | Phase | Description |
|---------|-------|-------------|
| `/aidlc` | - | Main orchestrator (start here) |
| `/aidlc-workspace-detection` | INCEPTION 1 | Scan workspace, detect greenfield/brownfield |
| `/aidlc-reverse-engineering` | INCEPTION 2 | Analyze existing codebase (brownfield) |
| `/aidlc-requirements-analysis` | INCEPTION 3 | Gather and analyze requirements |
| `/aidlc-user-stories` | INCEPTION 4 | Create user stories with INVEST criteria |
| `/aidlc-workflow-planning` | INCEPTION 5 | Determine execution plan |
| `/aidlc-application-design` | INCEPTION 6 | Component and service layer design |
| `/aidlc-units-generation` | INCEPTION 7 | Decompose system into units |
| `/aidlc-functional-design` | CONSTRUCTION 1 | Business logic design (per-unit) |
| `/aidlc-nfr-requirements` | CONSTRUCTION 2 | Non-functional requirements (per-unit) |
| `/aidlc-nfr-design` | CONSTRUCTION 3 | NFR pattern design (per-unit) |
| `/aidlc-infrastructure-design` | CONSTRUCTION 4 | Infrastructure mapping (per-unit) |
| `/aidlc-code-generation` | CONSTRUCTION 5 | Code generation (per-unit) |
| `/aidlc-build-and-test` | CONSTRUCTION 6 | Build and test instructions |
| `/aidlc-operations` | OPERATIONS | Placeholder for future release |

## Agents

Each command delegates to a specialized agent via the Task tool. Agents use the fully qualified `aidlc-for-claude:` prefix and are tiered by model for cost-efficiency:

### Opus (Strategic Reasoning)

| Agent | Purpose |
|-------|---------|
| `aidlc-for-claude:aidlc-reverse-engineer` | Deep codebase analysis for brownfield projects |
| `aidlc-for-claude:aidlc-requirements-analyst` | Requirements gathering with adaptive depth |
| `aidlc-for-claude:aidlc-story-writer` | User story creation with INVEST criteria |
| `aidlc-for-claude:aidlc-workflow-planner` | Execution planning and stage determination |
| `aidlc-for-claude:aidlc-application-designer` | Component and service layer design |
| `aidlc-for-claude:aidlc-units-planner` | System decomposition into units |
| `aidlc-for-claude:aidlc-code-planner` | Code generation plan creation |

### Sonnet (Volume Work)

| Agent | Purpose |
|-------|---------|
| `aidlc-for-claude:aidlc-functional-designer` | Business logic and domain model design |
| `aidlc-for-claude:aidlc-nfr-analyst` | Non-functional requirements assessment |
| `aidlc-for-claude:aidlc-nfr-designer` | NFR pattern and component design |
| `aidlc-for-claude:aidlc-infra-designer` | Infrastructure service mapping |
| `aidlc-for-claude:aidlc-code-generator` | Code generation execution |
| `aidlc-for-claude:aidlc-build-test-engineer` | Build and test instruction generation |

### Haiku (Fast Detection)

| Agent | Purpose |
|-------|---------|
| `aidlc-for-claude:aidlc-workspace-analyst` | Workspace scanning and project type detection |

**Model strategy:** Opus handles stages requiring deep reasoning (requirements analysis, architectural decisions, planning). Sonnet handles volume work (design documents, code generation, testing). Haiku handles fast detection (workspace scanning, project classification).

## Key Conventions

**Interactive Q&A** -- Requirements are gathered via Claude Code's interactive question UI with clickable multiple-choice options. All decisions are documented in the audit trail.

**Approval gates** are required at each stage. You review the generated artifacts and choose to approve, request changes, or add/remove stages.

**Audit trail** (`aidlc-docs/audit.md`) captures every user input with ISO 8601 timestamps. It is append-only and never overwritten.

**Session continuity** is handled via `aidlc-docs/aidlc-state.md`. Re-running `/aidlc` detects existing state and offers to resume.

**Adaptive depth** means all defined artifacts for a stage are created, but the detail level adapts to problem complexity. A simple bug fix gets concise artifacts; a complex system gets comprehensive treatment.

**ASCII diagrams** use only `+`, `-`, `|`, `^`, `v`, `<`, `>` characters. No Unicode box-drawing characters.

## Artifacts

All documentation is generated in the `aidlc-docs/` directory. Application code is always placed in the workspace root.

```
aidlc-docs/
  aidlc-state.md                    # Workflow state tracking
  audit.md                          # Append-only audit trail
  inception/
    plans/                          # Execution plans
    reverse-engineering/            # 8 RE artifacts (brownfield)
    requirements/                   # Requirements + question files
    user-stories/                   # Stories + personas
    application-design/             # Components, services, dependencies
  construction/
    plans/                          # Per-unit code plans
    {unit-name}/
      functional-design/            # Business logic, rules, entities
      nfr-requirements/             # Quality attributes, tech stack
      nfr-design/                   # Patterns, logical components
      infrastructure-design/        # Service mapping
      code/                         # Code summaries (code itself at workspace root)
    build-and-test/                 # Build and test instructions
```

## Plugin Structure

```
aidlc-for-claude/
  .claude-plugin/
    plugin.json                     # Plugin metadata
    marketplace.json                # Marketplace listing
  commands/                         # 15 slash commands
    aidlc.md                        # Entry point orchestrator
    aidlc-workspace-detection.md
    aidlc-reverse-engineering.md
    aidlc-requirements-analysis.md
    aidlc-user-stories.md
    aidlc-workflow-planning.md
    aidlc-application-design.md
    aidlc-units-generation.md
    aidlc-functional-design.md
    aidlc-nfr-requirements.md
    aidlc-nfr-design.md
    aidlc-infrastructure-design.md
    aidlc-code-generation.md
    aidlc-build-and-test.md
    aidlc-operations.md
  agents/                           # 14 specialized agents
    aidlc-workspace-analyst.md
    aidlc-reverse-engineer.md
    aidlc-requirements-analyst.md
    aidlc-story-writer.md
    aidlc-workflow-planner.md
    aidlc-application-designer.md
    aidlc-units-planner.md
    aidlc-functional-designer.md
    aidlc-nfr-analyst.md
    aidlc-nfr-designer.md
    aidlc-infra-designer.md
    aidlc-code-planner.md
    aidlc-code-generator.md
    aidlc-build-test-engineer.md
  docs/                             # GitHub Pages (Material for MkDocs)
  .github/workflows/                # CI/CD (docs deployment)
  .gitignore
  package.json
  LICENSE
  CONTRIBUTING.md
  README.md
```

## Origin

AI-DLC was originally developed as an AI-driven software development methodology by AWS. This plugin adapts the methodology for Claude Code's native command/agent system:

- Kiro's rule-loading mechanism becomes self-contained agent markdown files
- Kiro's file-based approvals become interactive approval gates
- Single-AI execution becomes multi-agent delegation with model tiering (Opus/Sonnet/Haiku)

### References

- [AI-DLC Methodology Blog (AWS Tech Blog)](https://aws.amazon.com/ko/blogs/tech/ai-driven-development-life-cycle/) - Original methodology introduction
- [AI-DLC Interactive Demo](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) - Live demo of the AI-DLC workflow
- [AI-DLC Workflows for Kiro (GitHub)](https://github.com/awslabs/aidlc-workflows) - Original Kiro steering files

## License

Apache-2.0
