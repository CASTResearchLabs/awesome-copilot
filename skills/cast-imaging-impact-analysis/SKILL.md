---
name: cast-imaging-impact-analysis
description: 'Assess change impact, analyze risks, and develop testing strategies using CAST Imaging structural analysis.'
---

# CAST Imaging Impact Analysis Skill

Assess change impact, analyze risks, and develop testing strategies using CAST Imaging structural analysis.

## Purpose

This skill helps you understand the cascading effects of code changes, evaluate risks, identify affected business flows, and create targeted testing plans. Use it before making changes to understand impact scope, or after changes to validate affected areas.

## Available Functions

**Object Relationships:**

- `object_details` (multiple modes) - Comprehensive object analysis
- `transactions_using_object` - Find transactions that use specific objects
- `data_graphs_involving_object` - Find data graphs involving specific objects

**Cross-Application Impact:**

- `inter_applications_dependencies` - Dependencies with other applications
- `inter_app_detailed_dependencies` - Detailed inter-application dependencies
- `applications_dependencies` - Portfolio-wide dependencies

**Quality Risk Assessment:**

- `quality_insights` - Quality issues that may affect changes
- `quality_insight_occurrences` - Specific locations of quality issues

## Object Details Modes

The `object_details` function supports these focus modes:

| Mode | Focus | Returns | Use Case |
|------|-------|---------|----------|
| `intra` | Object properties and children | Basic properties, child objects, signatures | Understanding object structure |
| `inward` | Incoming flows (callers) | Objects that call this one | Dependencies and impact |
| `outward` | Outgoing flows (callees) | Objects this one calls | What this object depends on |
| `testing` | Transaction/data graph usage | Which transactions and data graphs use this object | Assessing scope of changes |
| `code` | Code snippets | Actual code if available | Reading implementation |
| `insights` | Quality issues | CVEs, vulnerabilities, code smells | Identifying problems |
| `documents` | Documentation | Post-its and annotations | Reading context and notes |

## Recommended Workflows

### Change Impact Assessment

For comprehensive analysis of potential changes:

```text
objects (find the target objects)
→ object_details (mode=testing) - see which business flows are affected
→ transactions_using_object - list affected transactions
→ data_graphs_involving_object - list affected data flows
→ inter_app_detailed_dependencies - check cross-application impact
```

### Risk Assessment

For evaluating quality risks of changes:

```text
quality_insights - identify existing issues in the area
→ quality_insight_occurrences - locate specific issues
→ transaction_details (mode=insights) - quality issues in affected transactions
→ object_details (mode=insights) - quality issues in target objects
```

### Cross-Application Impact

For analyzing impacts across the enterprise:

```text
applications_dependencies - portfolio-wide view
→ inter_applications_dependencies - specific application dependencies
→ applications_quality_insights - quality issues across applications
→ applications_transactions - affected transactions across applications
```

### Testing Strategy Development

For creating targeted testing plans:

```text
transactions_using_object - identify affected transactions
→ data_graphs_involving_object - identify affected data flows
→ transaction_details (mode=type_graph) - understand transaction structure
→ quality_insights - identify areas needing extra testing attention
```

## Object Identification Filters

Use filters to find objects:

| Field | Operators | Example |
|-------|-----------|---------|
| `name` | contains | `name:contains:UserService` |
| `fullname` | contains | `fullname:contains:com.example.UserService` |
| `type` | contains | `type:contains:method` |
| `filepath` | contains | `filepath:contains:controller` |
| `id` | eq | `id:eq:12345` (takes precedence) |

**Compound term parsing** for natural queries:

- "catalog table" → `name:contains:catalog,type:contains:table`
- "OrderController class" → `name:contains:OrderController,type:contains:class`
- "getUser method" → `name:contains:getUser,type:contains:method`

## Common Questions This Skill Answers

- "What would be impacted if I change this component?"
- "Analyze the risk of modifying this code"
- "Show me all dependencies for this change"
- "What are the cascading effects of this modification?"
- "What quality risks are associated with this change?"
- "How does this change interact with existing technical debt?"
- "How will this change affect other applications?"
- "What cross-application impacts should I consider?"
- "What testing should I do for this change?"
- "How should I validate this modification?"
- "Create a testing plan for this impact area"
- "What calls the authenticate method?"
- "What does the processPayment function call?"

## Impact Analysis Checklist

Before making changes, verify:

**1. Direct Dependencies**

- [ ] What objects call this code? (`object_details` mode=inward)
- [ ] What does this code call? (`object_details` mode=outward)

**2. Business Flow Impact**

- [ ] Which transactions are affected? (`transactions_using_object`)
- [ ] Which data flows are affected? (`data_graphs_involving_object`)

**3. Cross-Application Impact**

- [ ] Are other applications dependent? (`inter_applications_dependencies`)
- [ ] What are the detailed dependencies? (`inter_app_detailed_dependencies`)

**4. Quality Considerations**

- [ ] Are there existing quality issues? (`object_details` mode=insights)
- [ ] Will the change affect high-risk areas? (`quality_insights`)

**5. Testing Scope**

- [ ] What transactions need testing? (from step 2)
- [ ] What data flows need validation? (from step 2)
- [ ] Are there quality issues requiring extra attention? (from step 4)

## Best Practices

1. **Start with testing mode**: Use `object_details` (mode=testing) first to understand scope
2. **Check both directions**: Analyze both inward (callers) and outward (callees) dependencies
3. **Consider business context**: Prioritize impact on business-critical transactions
4. **Cross-reference quality**: Overlay quality insights on impact analysis
5. **Think enterprise-wide**: Check cross-application dependencies for shared components
6. **Document findings**: Create testing plans based on affected flows
