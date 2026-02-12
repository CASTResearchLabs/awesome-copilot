---
name: cast-imaging-business-flow
description: 'Analyze business processes, transactions, data flows, and generate summaries using CAST Imaging structural analysis.'
---

# CAST Imaging Business Flow Skill

Analyze business processes, transactions, data flows, and generate summaries using CAST Imaging structural analysis.

## Purpose

This skill helps you understand how business processes are implemented in code, trace data flows, identify workflow bottlenecks, and generate comprehensive summaries. Use it for business process documentation, data lineage analysis, and workflow optimization.

## Available Functions

**Transaction Analysis:**

- `transactions` - List transactions (API/UI endpoints) in an application
- `transaction_details` - Comprehensive transaction analysis with multiple modes
- `transaction_profiles` - Statistical profiles of transactions

**Data Flow Analysis:**

- `data_graphs` - List data entity interaction networks
- `data_graph_details` - Comprehensive data graph analysis with multiple modes
- `data_graph_profiles` - Statistical profiles of data graphs

**Cross-System Analysis:**

- `applications_transactions` - Search transactions across applications
- `applications_data_graphs` - Search data graphs across applications

**Optional (if available):**

- `graph_intersection_analysis` - Analyze sharing patterns to distinguish owned business logic from reusable infrastructure

## Transaction & Data Graph Modes

Both `transaction_details` and `data_graph_details` support these modes:

| Mode | Purpose | Use Case |
|------|---------|----------|
| `type_graph` | **Start here** - Nodes/links aggregated by object types | Architectural overview |
| `focus_graph` | Filtered view: entry point + top N complex + top N with insights + leaf nodes | Quick hotspot identification |
| `graph` | Complete call/data graph with all nodes and links | Full analysis |
| `nodes` | All objects (no links) | Inventory of code involved |
| `links` | All connections (no node details) | Understand flow |
| `complexity` | Only complex objects | Refactoring candidates |
| `insights` | Only objects with quality/security issues | Bug fixing, security review |
| `centrality` | Object reuse profile across graphs | Identify shared utilities |
| `documents` | Attached documentation (post-its) | Review business context |
| `summary` | AI-generated summary | Quick understanding |

## Recommended Workflows

### Business Flow Discovery

When understanding business processes:

```text
transactions
→ transaction_details (mode=type_graph)
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary) to check sharing profiles
→ [fallback if not available]:
    → transaction_details (mode=centrality) to identify reused components
→ transaction_details (mode=insights)
```

If transactions are bloated/complex, distinguish whether complexity is due to:

- **Actual business logic** (low sharing/centrality = owned code)
- **Missing service boundaries** (high sharing/centrality = reusable infrastructure)

### Data Flow Analysis

When tracing data through the system:

```text
data_graphs
→ data_graph_details (mode=type_graph)
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary)
→ [fallback if not available]:
    → data_graph_details (mode=centrality)
→ data_graph_details (mode=insights)
```

### Workflow Optimization

When identifying bottlenecks:

```text
transactions
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary) - check for coupling patterns first
    → Look for identical intersection sizes
    → graph_intersection_analysis (mode=details) - validate candidates
→ [fallback if not available]:
    → transaction_details (mode=centrality) for multiple transactions - compare patterns
→ transaction_details (mode=complexity)
→ transaction_details (mode=focus_graph)
```

### Cross-System Integration

When business processes span multiple applications:

```text
applications_transactions → inter_applications_dependencies → applications_data_graphs
```

### AI Summary Generation

When creating summaries for transactions/data graphs without existing summaries:

```text
Check mode=summary first
→ If missing/insufficient:
    → transaction_details/data_graph_details (mode=type_graph) - architectural overview
    → mode=complexity - identify complex operations
    → mode=centrality - understand reuse patterns
    → [if graph_intersection_analysis available]:
        → graph_intersection_analysis (mode=summary) - identify owned vs reused code
→ Synthesize into structured summary
```

**Output format for summaries (Imaging Viewer structure):**

- Functional Explanation
- Technical Explanation
- Architectural Coupling (owned vs reused breakdown)
- Objects of Interest

## Common Questions This Skill Answers

- "Show me the business processes in this application"
- "How does the user registration flow work?"
- "What are the main business workflows?"
- "Map out the order processing flow"
- "Why is this business process so complex?"
- "How does customer data flow through the system?"
- "Trace the data lineage for this entity"
- "What data transformations happen in this flow?"
- "Where are the bottlenecks in our business processes?"
- "Which workflows are most complex?"
- "How can we optimize this business flow?"
- "Create an AI summary for this transaction"
- "Summarize what this data graph does"
- "What's unique about this transaction vs infrastructure code?"

## Interactive Visualization

When users want customized graph visualizations:

1. **Start with** `graph_intersection_analysis` (mode=summary) if available, or `transaction_details` (mode=centrality) to identify sharing profiles
2. **Categorize nodes** by reuse level:
   - OWNED (low sharing) - unique business logic
   - SHARED (medium sharing) - cross-cutting concerns
   - INFRASTRUCTURE (high sharing) - candidates for extraction
3. **Choose visualization strategy**:
   - `mode=type_graph` for exploration
   - `mode=focus_graph` for refactoring focus
   - `mode=insights` for quality audit
4. **Transform** complex graphs (100+ nodes) into readable visualizations (20-40 nodes)

## Best Practices

1. **Start with type_graph**: Get architectural overview before diving into details
2. **Use focus_graph for hotspots**: Quickly identify problem areas without overwhelming detail
3. **Distinguish owned vs shared**: Use intersection analysis if available, or centrality mode as fallback
4. **Consider business value**: Prioritize analysis of business-critical flows
5. **Document findings**: Generate summaries for future reference
