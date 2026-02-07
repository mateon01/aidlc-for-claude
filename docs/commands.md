---
title: Commands & Agents
layout: default
nav_order: 4
---

# Commands & Agents
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

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
| `/aidlc-functional-design` | 1 | Business logic design (per-unit) |
| `/aidlc-nfr-requirements` | 2 | Non-functional requirements (per-unit) |
| `/aidlc-nfr-design` | 3 | NFR pattern design (per-unit) |
| `/aidlc-infrastructure-design` | 4 | Infrastructure mapping (per-unit) |
| `/aidlc-code-generation` | 5 | Code generation (per-unit, two-part) |
| `/aidlc-build-and-test` | 6 | Build and test instructions |

### OPERATIONS Phase

| Command | Description |
|:--------|:------------|
| `/aidlc-operations` | Placeholder for future release |

---

## Agents

Each command delegates to a specialized agent. Agents are tiered by model for cost-efficiency.

### Opus Agents (Strategic Reasoning)

| Agent | Purpose |
|:------|:--------|
| `aidlc-reverse-engineer` | Deep codebase analysis for brownfield projects |
| `aidlc-requirements-analyst` | Requirements gathering with adaptive depth |
| `aidlc-story-writer` | User story creation with INVEST criteria |
| `aidlc-workflow-planner` | Execution planning and stage determination |
| `aidlc-application-designer` | Component and service layer design |
| `aidlc-units-planner` | System decomposition into implementation units |
| `aidlc-code-planner` | Code generation plan creation |

### Sonnet Agents (Volume Work)

| Agent | Purpose |
|:------|:--------|
| `aidlc-workspace-analyst` | Workspace scanning and project type detection |
| `aidlc-functional-designer` | Business logic and domain model design |
| `aidlc-nfr-analyst` | Non-functional requirements assessment |
| `aidlc-nfr-designer` | NFR pattern and component design |
| `aidlc-infra-designer` | Infrastructure service mapping |
| `aidlc-code-generator` | Code generation execution |
| `aidlc-build-test-engineer` | Build and test instruction generation |

---

## Two-Part Stages

Three stages use a two-part approach for quality assurance:

| Stage | Part 1 (Plan) | Part 2 (Execute) |
|:------|:--------------|:-----------------|
| User Stories | Story plan with personas | Individual story generation |
| Units Generation | Decomposition plan | Unit definition documents |
| Code Generation | File-by-file code plan (Opus) | Actual code generation (Sonnet) |

In each case, you approve the plan before execution begins.
