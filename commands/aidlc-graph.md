---
description: "AI-DLC: Graph Analysis - Build, update, visualize, and repair via code dependency graphs"
---

# AI-DLC: Graph Analysis

Standalone utility for code dependency graph operations. Can be run independently of the three-phase workflow at any time.

## Usage

This command delegates to the graph analyzer agent for on-demand graph operations:

```
Task(subagent_type="aidlc-for-claude:aidlc-graph-analyzer", model="sonnet",
     prompt="Run graph analysis in standalone mode. Ask the user which operation they want: build, update, visualize, export, impact analysis, search, repair, verify, or teardown. Then execute the selected operation on the current workspace.")
```

## Modes

- **Build graph** — Full static analysis of codebase to construct dependency graph
- **Update graph** — Incrementally update existing graph with recent changes
- **Visualize** — Generate Mermaid diagram from existing graph
- **Export as PNG** — Export dependency graph as PNG images (requires Python matplotlib + networkx)
- **Impact analysis** — Show affected modules based on recent file changes
- **Search (GraphRAG)** — Find modules by semantic query using summaries and graph context (requires graphRAGEnabled)
- **Repair (CGIG)** — Parse compilation errors, query graph for repair context, return fix suggestions with confidence scores (requires cgigEnabled)
- **Verify** — Run deployment verification on graph DB
- **Teardown** — Stop graph DB container or clean up resources
