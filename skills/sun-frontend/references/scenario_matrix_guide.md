# Scenario Matrix Guide

Version: v1.0.0  
Purpose: Define structured self-test validation matrix for Phase 3.  
Scope: React + TypeScript enterprise frontend projects.

---

# 1. Why Scenario Matrix Exists

Self-Test phase is not casual verification.

It must:

- Simulate QA thinking
- Simulate Code Review thinking
- Detect hidden state issues
- Detect side-effect pollution
- Detect regression risk

Scenario Matrix ensures structured coverage.

---

# 2. Validation Dimensions

Each feature must be validated across these dimensions:

1. Functional Path
2. Boundary Conditions
3. Exception Path
4. State Consistency
5. Hook Dependency Integrity
6. Reusability & Coupling
7. Regression Impact

---

# 3. Functional Path Validation

Validate:

- Initial render behavior
- User interaction sequence
- Data request triggering
- State updates
- UI re-render correctness

Questions:

- Is every user action deterministic?
- Are transitions predictable?
- Are loading states handled correctly?

---

# 4. Boundary Conditions

Validate:

- Empty data
- Null / undefined values
- Maximum length inputs
- Minimum length inputs
- Large dataset rendering
- Slow network simulation

Prohibited:

- Assuming backend always returns valid data
- Ignoring undefined checks

---

# 5. Exception Path Validation

Must simulate:

- API failure
- Timeout
- Permission denial
- Partial data response
- Unexpected data structure

Check:

- Error UI rendering
- Retry logic
- State reset behavior
- Toast / notification correctness

---

# 6. State Consistency Validation

Evaluate:

- Is there duplicated derived state?
- Is state updated in multiple places?
- Are there race conditions?
- Are stale closures possible?
- Is useEffect dependency correct?

If any inconsistency is detected,
document and re-evaluate design.

---

# 7. Hook Dependency Integrity

For each hook:

- Verify dependency array completeness
- Verify no hidden mutable reference
- Verify memoization necessity
- Verify cleanup logic

Common risks:

- Missing dependency
- Infinite render loop
- Async race condition
- Memory leak

---

# 8. Reusability & Coupling

Validate:

- Component over-coupling
- Prop drilling explosion
- Hidden cross-module dependency
- Hard-coded logic

Ask:

- Can this component be reused?
- Is responsibility boundary clear?
- Does this violate governance baseline?

---

# 9. Regression Impact Evaluation

Must identify:

- Affected modules
- Shared component usage
- Shared hook usage
- API contract consumers

Regression risk levels:

- Isolated
- Module-level
- Cross-module
- Global

---

# 10. Required Self-Test Output Structure

Phase 3 report must include:

1. Feature summary
2. Functional path coverage list
3. Boundary validation results
4. Exception scenario results
5. State consistency evaluation
6. Hook dependency verification
7. Regression impact statement
8. Final confidence level (High / Medium / Low)

---

# 11. Confidence Level Definition

High:

- All dimensions validated
- No structural issue
- No race condition
- No shared impact risk

Medium:

- Minor warnings
- Low impact refactor risk

Low:

- State inconsistency detected
- Shared component impact unclear
- Risk of regression not fully verified

Low confidence requires returning to Phase 2.

---

# Summary

Self-Test is structural validation,
not manual clicking.

Every validation must be documented.

No undocumented assumption is allowed.