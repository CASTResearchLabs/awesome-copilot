---
name: cast-imaging-architecture
description: 'Analyze application architecture, dependencies, coupling patterns, and plan modernization strategies using CAST Imaging structural analysis.'
---

# CAST Imaging Architecture Skill

Analyze application architecture, dependencies, coupling patterns, and plan modernization strategies using CAST Imaging structural analysis.

## Purpose

This skill helps you understand architectural coupling, plan service extractions, identify missing service boundaries, and create data-driven modernization roadmaps. Use it for microservices decomposition, SOA initiatives, and architectural evolution planning.

## Available Functions

**Architecture Analysis:**

- `architectural_graph` - View application architecture at various levels (layer, component, sub-component, technology-category, element-type)
- `graph_intersection_analysis` *(if available)* - Analyze shared/coupled code between transactions or data graphs (critical for modernization)

**Dependency Analysis:**

- `inter_applications_dependencies` - Inter-application dependencies for an application
- `inter_app_detailed_dependencies` - Detailed dependencies between two applications
- `applications_dependencies` - Portfolio-wide inter-application dependencies

**Modernization Support:**

- `transaction_details` (mode=complexity) - Find complex code for refactoring
- `data_graph_details` (mode=complexity) - Find complex data operations
- `object_details` (mode=testing) - Understand impact of extracting objects

## Recommended Workflows

### Modernization & Refactoring Strategy

When planning service extractions or refactoring:

```text
transactions OR data_graphs
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary or mode=top_n) - identify high-sharing candidates
    → graph_intersection_analysis (mode=details) - validate by examining actual objects and links
    → Look for identical intersection sizes (same objects pattern)
→ [fallback if not available]:
    → transaction_details (mode=centrality) - identify reused objects
    → object_details (mode=testing) - check usage across transactions
→ transaction_details OR data_graph_details (mode=complexity)
→ transaction_details OR data_graph_details (mode=insights)
```

### Service Extraction Planning

For identifying shared code to extract into services:

```text
transactions
→ transaction_details (mode=type_graph) - overview
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary) - identify coupling patterns
    → Look for identical intersection sizes across multiple peers
    → graph_intersection_analysis (mode=details) for 2-3 candidates - validate if same objects
→ [fallback if not available]:
    → transaction_details (mode=centrality) - find highly reused objects
→ transaction_details (mode=complexity) - validate complexity
→ object_details (mode=testing) - check usage and impact
```

### Architecture Evolution Planning

For strategic modernization guidance:

```text
architectural_graph - overview
→ transactions - identify bloated ones
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary) - find coupling/missing services
    → Look for identical intersection sizes
    → graph_intersection_analysis (mode=details) - validate candidates
→ [fallback if not available]:
    → transaction_details (mode=centrality) for each bloated transaction
→ transaction_details (mode=complexity)
→ quality_insights
```

### Integration Discovery

For understanding integration patterns:

```text
applications → inter_applications_dependencies → inter_app_detailed_dependencies
```

## Understanding graph_intersection_analysis

> **Note:** This function may not be available in all Imaging MCP server versions. Check availability before using. When unavailable, use `transaction_details` (mode=centrality) and `object_details` (mode=testing) as alternatives.

This is the key function for modernization decisions. It reveals:

- **Owned code** (low sharing): Unique to this component - actual business logic
- **Shared code** (high sharing): Used across many components - extraction candidates
- **Identical intersection sizes**: Strongly suggests the SAME objects are shared - potential service boundary

**Modes:**

- `summary` - Start here: statistics (min/max/avg/median) and sorted list of all intersections
- `top_n` - Quick scan of most significant intersections
- `bottom_n` - Find loosely coupled components
- `details` - Deep dive: full sharing matrix with object lists and links

## Common Questions This Skill Answers

- "How should we modernize this application?"
- "Which services should we extract first?"
- "What's the refactoring priority for this bloated transaction?"
- "Plan service extraction with ROI analysis"
- "Identify architectural coupling for modernization"
- "Which components should be extracted into microservices?"
- "What code is shared between this transaction and others?"
- "Which objects should I extract into a service?"
- "Find missing service boundaries in the architecture"
- "How should we evolve our architecture?"

## Validation Requirements

Before recommending extractions, always:

1. **Examine actual objects** in intersection details mode (or via centrality/testing modes)
2. **Check object types and links** to determine extraction feasibility
3. **Assess whether shared objects are infrastructure code** (good candidates) or domain utilities (may be properly structured)
4. **Consider extraction complexity vs. business value** - widely invoked code may have many dependencies
5. **Understand call paths** (direct calls vs. deep in call chains) using details mode

## Common Pitfalls to Avoid

- Don't extract based on complexity alone without checking sharing patterns first
- High sharing doesn't automatically mean extraction is beneficial - validate the actual objects
- Widely invoked code may have many dependencies making extraction difficult
- Some shared invocations may not warrant extraction (e.g., legitimate shared libraries)
- Identical intersection sizes across multiple peers often indicate the SAME objects - verify before creating multiple services
