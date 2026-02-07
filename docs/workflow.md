---
title: Workflow
layout: default
nav_order: 3
---

# Workflow Details
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## INCEPTION Phase (WHAT and WHY)

The Inception phase establishes what needs to be built and why. It consists of 7 stages.

### Stage 1: Workspace Detection

Scans the workspace to determine if it's greenfield (new project) or brownfield (existing code). This decision affects which subsequent stages are executed.

### Stage 2: Reverse Engineering (Brownfield Only)

For existing codebases, performs deep analysis producing 8 artifacts:

| Artifact | Content |
|:---------|:--------|
| Business Overview | Domain context and purpose |
| Architecture Map | System architecture and patterns |
| Code Structure | Directory layout and organization |
| API Inventory | Endpoints, interfaces, contracts |
| Component Inventory | Modules, services, libraries |
| Tech Stack | Languages, frameworks, dependencies |
| Dependency Graph | Internal and external dependencies |
| Code Quality | Patterns, anti-patterns, tech debt |

### Stage 3: Requirements Analysis

Gathers and analyzes requirements using file-based questionnaires. Depth adapts to complexity -- simple bug fixes get minimal questions, complex systems get thorough analysis.

### Stage 4: User Stories (Conditional)

Creates user stories following INVEST criteria (Independent, Negotiable, Valuable, Estimable, Small, Testable). Includes persona definition and acceptance criteria.

### Stage 5: Workflow Planning

The critical decision stage. Evaluates all gathered context and determines which subsequent stages are needed. You can override any recommendation.

### Stage 6: Application Design (Conditional)

Designs component architecture, service layers, and inter-service dependencies. Produces design documents with ASCII diagrams.

### Stage 7: Units Generation (Conditional)

Decomposes the system into implementation units. Each unit becomes the scope for the Construction phase's per-unit loop.

---

## CONSTRUCTION Phase (HOW)

The Construction phase implements the system. For each unit, it runs a loop of 4-5 stages, followed by a final build and test stage.

### Per-Unit Loop

For each unit defined in Inception:

**Step 1: Functional Design** -- Business logic, domain models, rules, and entities specific to this unit.

**Step 2: NFR Requirements** -- Quality attributes (performance, security, reliability) and technology stack decisions for this unit.

**Step 3: NFR Design** -- Patterns and logical components that satisfy the non-functional requirements.

**Step 4: Infrastructure Design** -- Maps logical components to infrastructure services (databases, caches, queues, etc.).

**Step 5: Code Generation** -- Two-part stage:
1. **Planning** (Opus) -- Creates a detailed code generation plan with file-by-file breakdown
2. **Execution** (Sonnet) -- Generates actual code following the plan

### Build and Test

After all units are complete, generates build instructions and test plans for the entire system.

---

## OPERATIONS Phase (Future)

Placeholder for deployment, monitoring, and operational concerns. Will be implemented in a future release.

---

## Stage Execution Matrix

| Stage | Simple Bug Fix | New Feature | Brownfield | Infra Change |
|:------|:-:|:-:|:-:|:-:|
| Workspace Detection | Always | Always | Always | Always |
| Reverse Engineering | - | - | Yes | - |
| Requirements Analysis | Always | Always | Always | Always |
| User Stories | - | Yes | Conditional | - |
| Workflow Planning | Always | Always | Always | Always |
| Application Design | - | Yes | Conditional | - |
| Units Generation | - | Yes | Conditional | - |
| Functional Design | - | Yes | Conditional | - |
| NFR Requirements | - | Yes | Conditional | - |
| NFR Design | - | Yes | Conditional | - |
| Infrastructure Design | - | Yes | Conditional | Yes |
| Code Generation | Always | Always | Always | Always |
| Build and Test | Always | Always | Always | Always |

---

## Model Strategy

AI-DLC uses two model tiers for cost-efficiency:

| Model | Role | Used For |
|:------|:-----|:---------|
| **Opus** | Strategic reasoning | Requirements analysis, architectural decisions, planning, system decomposition |
| **Sonnet** | Volume execution | Functional design, NFR analysis, code generation, testing |
