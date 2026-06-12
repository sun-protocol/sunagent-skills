# frontend-workflow

Frontend Engineering Execution Workflow Engine

This skill governs structured frontend task execution with internal phase reasoning and a single final AuditReport output.

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

# Output Policy

Only produce one final report artifact by default:

`AuditReport.md`

Planning, development design, self-test, and PR gate analysis must be performed as internal working notes unless the user explicitly asks for phase documents.

Do not create Blueprint directories or full per-phase files by default.

---

# Phase 1: Planning

## Objective

Generate structured frontend technical solution.

## Internal Analysis Structure (Mandatory if strict=on)

1. Requirement Overview
2. Functional Module Decomposition
3. Data Contract Definition
4. State & Interaction Design
5. Component Architecture
6. Technical Decision Record (if applicable)
7. Risk & Edge Case Analysis
8. Execution Plan

## Rules

- Must be structured internally.
- Must define data contracts clearly.
- Must analyze state transitions.
- Must identify performance and memory risks.
- May be skipped only for trivial tasks, but status must record as Skipped in AuditReport.

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

AuditReport format:

Self-Test Result: PASS | PASS_WITH_RISK | BLOCK

Risk Level:
P0 | P1 | P2 | P3

Blocking Issues:
- item

Recommendations:
- item

If BLOCK → recommend no PR merge.

---

# AuditReport Requirements

The final `AuditReport.md` must include:

1. Requirement Summary
2. Change Summary
3. Impacted Files / Modules
4. State / API / Component Impact
5. Risk Level and Rationale
6. Blocking Issues, if any
7. Verification Summary
8. Recommendations / Follow-up
9. Phase Status Tracker

Keep the report concise. Omit full scenario matrices, full component trees, and repeated checklist detail unless they are needed to explain a concrete risk.

---

# Phase Status Tracker

Maintain:

Planning: Done / Skipped  
Development: Done  
Self-Test: Done / Blocked  
PR Gate: Done / Blocked

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
- resources/audit_report_template.md

---

# Enforcement Principles

- Never skip structured reasoning.
- Never introduce unsafe modifications.
- Never produce large unreviewable diffs.
- Always prioritize correctness over speed.
- Always output one concise final AuditReport unless phase-specific documents are explicitly requested.

---

This skill acts as a frontend engineering governance engine.
