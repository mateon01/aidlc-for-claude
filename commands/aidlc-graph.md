---
description: "AI-DLC: Graph Analysis - Build, update, and visualize code dependency graphs"
---

# AI-DLC: Graph Analysis

Standalone utility for code dependency graph operations. Can be run independently of the three-phase workflow at any time.

## Usage

This command delegates to the graph analyzer agent for on-demand graph operations:

```
Task(subagent_type="aidlc-for-claude:aidlc-graph-analyzer", model="sonnet",
     prompt="Run graph analysis in standalone mode. Ask the user which operation they want: build, update, visualize, impact analysis, or search. Then execute the selected operation on the current workspace.")
```

## Modes

- **Build graph** — Full static analysis of codebase to construct dependency graph
- **Update graph** — Incrementally update existing graph with recent changes
- **Visualize** — Generate Mermaid diagram from existing graph
- **Impact analysis** — Show affected modules based on recent file changes
- **Search (GraphRAG)** — Find modules by semantic query using summaries and graph context (requires graphRAGEnabled)
