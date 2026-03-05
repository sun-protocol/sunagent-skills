# frontend-workflow

Frontend Engineering Execution Workflow Engine

This skill governs structured frontend task execution across three controlled phases.

---

# Core Parameters

--phase=planning | development | self-test
--risk=on | off
--strict=on | off
--mode=explicit | auto

---

# Phase Controller

The skill must enter the selected phase directly.

If no phase is provided, ask user to specify.

---

# Phase 1: Planning

## Objective

Generate structured frontend technical solution.

## Output Structure (Mandatory if strict=on)

1. Requirement Overview
2. Functional Module Decomposition
3. Data Contract Definition
4. State & Interaction Design
5. Component Architecture
6. Technical Decision Record (if applicable)
7. Risk & Edge Case Analysis
8. Execution Plan

## Rules

- Must be structured.
- Must define data contracts clearly.
- Must analyze state transitions.
- Must identify performance and memory risks.
- May be skipped, but status must record as Skipped.

---

# Phase 2: Development

## Objective

Govern AI-assisted development under strict engineering constraints.

## Execution Model

Must enforce R → I → P → E → R flow:

Research  
Innovate  
Plan  
Execute  
Review  

No direct execution without plan.

---

## Governance Enforcement

### Code Rules

- TypeScript types must be complete.
- ESLint and Prettier compliance required.
- Minimum viable diff principle.
- No unverified dependency introduction.
- Unified error handling.
- API layer decoupled from UI.

### Architecture Rules

- Functional components only.
- TailwindCSS preferred.
- Global state traceable.
- Avoid direct DOM manipulation.
- State synchronization consistency.

### Execution Rules

- Multi-file changes must be batched.
- Each batch must remain runnable.
- Pause after each batch in explicit mode.
- Record rollback point.

---

## Optional Risk Scan (if --risk=on)

Evaluate:

- Core flow impact
- Global store mutation impact
- Routing impact
- Public component impact
- Performance regression risk

---

# Phase 3: Self-Test

## Objective

Perform structured validation before PR merge.

---

## Step 1: Scenario Matrix

Generate:

- All state branches
- All interaction paths
- Negative cases
- Boundary conditions
- Network failure cases
- Repeated trigger cases

---

## Step 2: Checklist Validation

Validate:

- Loading state
- Error state
- Null handling
- Memory leak risk
- Polling cleanup
- Duplicate request risk
- Mobile compatibility

---

## Step 3: Regression Impact Analysis

Check:

- Shared components
- Global state
- Tracking / analytics
- SEO / SSR
- Performance metrics

---

## Step 4: PR Gate Decision

Output format:

Self-Test Result: PASS | PASS_WITH_RISK | BLOCK

Risk Level:
P0 | P1 | P2 | P3

Blocking Issues:
- item

Recommendations:
- item

If BLOCK → recommend no PR merge.

---

# Phase Status Tracker

Maintain:

Planning: Done / Skipped  
Development: Done  
Self-Test: Done / Blocked  

---

# References

- references/react_ts_governance.md
- references/risk_level_definition.md
- references/scenario_matrix_guide.md

---

# Resources

- resources/technical_plan_template.md
- resources/self_test_report_template.md
- resources/pr_gate_output_template.md

---

# Enforcement Principles

- Never skip structured reasoning.
- Never introduce unsafe modifications.
- Never produce large unreviewable diffs.
- Always prioritize correctness over speed.
- Always output structured results in planning and self-test phases.

---

This skill acts as a frontend engineering governance engine.