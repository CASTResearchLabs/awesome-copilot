---
name: cast-imaging-discovery
description: 'Explore and discover software applications, components, dependencies, and architectural patterns using CAST Imaging structural analysis.'
---

# CAST Imaging Discovery Skill

Explore and discover software applications, components, dependencies, and architectural patterns using CAST Imaging structural analysis.

## Purpose

This skill helps you understand what applications are available, their internal structure, dependencies, and design patterns. Use it when starting analysis of an unfamiliar codebase or exploring the software portfolio.

## Available Functions

**Application Exploration:**

- `applications` - List all available applications in Imaging
- `stats` - Get overview statistics for an application (size, technologies, complexity)
- `architectural_graph` - View application architecture at various levels (layer, component, sub-component, technology-category)

**Component Discovery:**

- `objects` - Find code objects by name, type, or path
- `object_details` (mode=intra) - Get object properties and children

**Dependency Mapping:**

- `packages` - List external packages/libraries used
- `package_interactions` - Get details about how packages are used
- `inter_applications_dependencies` - List dependencies between applications
- `applications_dependencies` - Portfolio-wide inter-application dependencies

**Pattern Identification:**

- `architectural_graph` - Identify recurring architectural patterns
- `quality_insights` - Find design pattern violations

## Recommended Workflows

### Application Discovery

When exploring available applications:

```text
applications → stats → architectural_graph
```

### Component Analysis

For understanding internal structure:

```text
stats → architectural_graph → objects → object_details (mode=intra)
```

### Dependency Mapping

For discovering dependencies at multiple levels:

```text
packages → package_interactions → inter_applications_dependencies → object_details (mode=outward)
```

### Business Context Integration

For connecting technical architecture to business workflows:

```text
transactions → transaction_details (mode=type_graph) → data_graphs → data_graph_details (mode=type_graph)
```

### Pattern Identification

For identifying architectural patterns and conventions:

```text
architectural_graph → objects → quality_insights → architectural_graph
```

## Common Questions This Skill Answers

- "What applications are available?"
- "Give me an overview of application X"
- "Show me the architecture of this application"
- "How is this application structured?"
- "What components does this application have?"
- "What dependencies does this application have?"
- "Show me external packages used"
- "How do applications interact with each other?"
- "What patterns are used in this application?"
- "Identify the architectural conventions"

## Best Practices

1. **Start broad, then focus**: Begin with `applications` and `stats` before diving into specifics
2. **Use multiple architectural levels**: Explore layer → component → sub-component → technology-category
3. **Cross-reference findings**: Validate discoveries using multiple tools
4. **Consider enterprise context**: Use portfolio-level tools (`applications_*`) when relevant
5. **Document discoveries**: Use terminology from `lingo` profiles to communicate findings
