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
| `/aidlc-code-generation` | 5 | Code generation (per-unit, two-part) |
| `/aidlc-build-and-test` | 6 | Build, test, and validate (actual execution with retry) |

### OPERATIONS Phase

| Command | Description |
|:--------|:------------|
| `/aidlc-operations` | Deployment checklist and developer README generation |

---

## Agents

Each command delegates to a specialized agent. Agents are tiered by model for cost-efficiency.

!!! note "Agent Naming"
    Agents use the fully qualified `aidlc-for-claude:` prefix when invoked via the Task tool.

### :material-brain: Opus Agents (Strategic Reasoning)

| Agent | Purpose |
|:------|:--------|
| `aidlc-for-claude:aidlc-reverse-engineer` | Deep codebase analysis for brownfield projects |
| `aidlc-for-claude:aidlc-requirements-analyst` | Requirements gathering with adaptive depth |
| `aidlc-for-claude:aidlc-story-writer` | User story creation with INVEST criteria |
| `aidlc-for-claude:aidlc-workflow-planner` | Execution planning and stage determination |
| `aidlc-for-claude:aidlc-application-designer` | Component and service layer design |
| `aidlc-for-claude:aidlc-units-planner` | System decomposition into implementation units |
| `aidlc-for-claude:aidlc-code-planner` | Code generation plan creation |

### :material-lightning-bolt: Sonnet Agents (Volume Work)

| Agent | Purpose |
|:------|:--------|
| `aidlc-for-claude:aidlc-functional-designer` | Business logic and domain model design |
| `aidlc-for-claude:aidlc-system-nfr-analyst` | System-level NFR decisions for multi-unit projects |
| `aidlc-for-claude:aidlc-nfr-analyst` | Non-functional requirements assessment |
| `aidlc-for-claude:aidlc-nfr-designer` | NFR pattern and component design |
| `aidlc-for-claude:aidlc-infra-designer` | Infrastructure service mapping |
| `aidlc-for-claude:aidlc-code-generator` | Code generation execution |
| `aidlc-for-claude:aidlc-build-test-engineer` | Build and test execution with instruction generation |
| `aidlc-for-claude:aidlc-ops-generator` | Deployment checklist and developer README generation |

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
