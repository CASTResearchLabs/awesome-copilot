---
name: cast-imaging-quality
description: 'Analyze code quality, security vulnerabilities, technical debt, and compliance issues using CAST Imaging structural analysis.'
---

# CAST Imaging Quality Skill

Analyze code quality, security vulnerabilities, technical debt, and compliance issues using CAST Imaging structural analysis.

## Purpose

This skill helps you identify quality issues, security vulnerabilities, technical debt patterns, and prioritize remediation efforts. Use it for security audits, quality assessments, compliance verification, and technical debt management.

## Available Functions

**Quality Overview:**

- `quality_insights` - List quality issues by type (CVE, cloud blockers, green deficiencies, structural flaws, ISO 5055)
- `applications_quality_insights` - Portfolio-wide quality insights across all applications

**Issue Details:**

- `quality_insight_occurrences` - Get specific occurrences of a quality issue
- `advisor_occurrences` - Get findings for specific advisors

**Advisors & Standards:**

- `advisors` - List all advisors (quality rules) for an application
- `iso_5055_explorer` - Explore ISO 5055 characteristics and weaknesses

**Context Analysis:**

- `object_details` (mode=insights) - Get quality issues for specific objects
- `transaction_details` (mode=insights) - Get quality issues in transactions
- `data_graph_details` (mode=insights) - Get quality issues in data flows

**Optional (if available):**

- `graph_intersection_analysis` - Identify if quality issues are in shared/coupled code (higher ROI when fixed)

## Recommended Workflows

### Quality Assessment

When analyzing application quality:

```text
quality_insights
→ quality_insight_occurrences
→ object_details (mode=insights)
→ [verify issue nature if unexpected results]
```

**Required in reports:**

1. Structural context analysis of where occurrences are located (packages, objects, layers)
2. Testing implications based on occurrence distribution
3. Explicit statement: "Source code is/is not available, so this analysis provides [detailed/high-level] guidance"
4. If occurrence query returns empty or unexpected results, re-verify the issue type

### Issue Prioritization

When deciding which issues to fix first:

```text
quality_insights
→ transaction_details (mode=insights)
→ data_graph_details (mode=insights)
→ [if graph_intersection_analysis available]:
    → graph_intersection_analysis (mode=summary) to check if issues are in shared code
    → mode=details to validate if issues are in shared objects
→ [fallback if not available]:
    → object_details (mode=testing) to check how many transactions use affected objects
```

Consider checking if issues are in highly coupled/shared code - fixing issues in shared code benefits more components.

### Root Cause Analysis

When investigating specific quality issues:

```text
quality_insight_occurrences
→ object_details (mode=insights)
→ transactions_using_object
→ [double-check issue nature if unexpected]
```

**Required in analyses:**

1. Structural context showing distribution of occurrences across architecture
2. Testing strategy focusing on affected transactions and data flows
3. Clear statement of source code access affecting analysis depth
4. Validation that occurrence data matches issue type - if not, investigate issue definition

### Quality Trend Analysis

For understanding quality patterns:

```text
quality_insights → objects → architectural_graph
```

## Quality Insight Types

Use the `nature` parameter to filter by type:

- `cve` - Known security vulnerabilities (CVEs)
- `cloud-detection-patterns` - Cloud migration blockers
- `green-detection-patterns` - Environmental/efficiency issues
- `structural-flaws` - Architectural and structural problems
- `iso-5055` - ISO 5055 standard violations (Security, Reliability, Performance Efficiency, Maintainability)

## Common Questions This Skill Answers

- "What quality issues are in this application?"
- "Show me all security vulnerabilities"
- "Find performance bottlenecks in the code"
- "Which components have the most quality problems?"
- "Which quality issues should I fix first?"
- "What are the most critical problems?"
- "Show me quality issues in business-critical components"
- "Why is this component flagged for quality issues?"
- "What's the impact of fixing this problem?"
- "Which parts of the application have the most issues?"
- "Are there quality patterns by component type?"

## Best Practices

1. **Always provide context**: Include structural location of issues in reports
2. **Consider business impact**: Prioritize issues in critical transactions/data flows
3. **Check sharing patterns**: Issues in shared code have higher ROI when fixed (use `graph_intersection_analysis` if available, or `object_details` mode=testing)
4. **Validate findings**: Cross-reference with multiple tools
5. **Document source code availability**: Analysis depth depends on code access
6. **Think testing implications**: Map issues to affected business flows for test planning
