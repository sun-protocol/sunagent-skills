# React + TypeScript Engineering Governance Baseline

Version: v1.0.0  
Scope: React + TypeScript enterprise frontend projects  
Purpose: Define the mandatory governance baseline for all phases.

---

# 1. Architectural Governance

## 1.1 Layering Rules

Frontend architecture must follow strict layering:
UI Layer
↓
Container / Page Layer
↓
Hooks Layer
↓
Service / API Layer
↓
Domain Model Layer

Rules:

- UI must not contain business logic.
- Hooks must isolate side effects.
- Service layer must not depend on UI.
- Domain model must be independent of framework.

---

## 1.2 Component Governance

Each component must define:

- Responsibility boundary
- Props contract (explicit interface)
- State ownership
- Side-effect ownership
- Reusability evaluation

Prohibited:

- God components
- Hidden shared state
- Uncontrolled prop spreading
- Business logic inside presentational components

---

# 2. Type System Governance

## 2.1 Strict Type Requirements

Mandatory:

- `"strict": true`
- `"noImplicitAny": true`
- `"strictNullChecks": true`

Prohibited:

- any
- implicit return types on exported functions
- implicit object shape inference for domain models

---

## 2.2 Domain Modeling Rules

Domain models must:

- Be declared explicitly
- Represent backend contract accurately
- Avoid partial implicit extension
- Avoid mixing UI state with domain state

Example separation:
UserDTO // API contract
UserModel // normalized domain model
UserViewState // UI state

---

# 3. State Management Governance

## 3.1 Local State Rules

- Avoid duplicated derived state
- Avoid redundant useEffect synchronization
- Avoid state mutation
- Always document ownership

---

## 3.2 Global State Rules

- Must justify necessity
- Must evaluate impact radius
- Must evaluate rollback complexity

Prohibited:

- Introducing global state for convenience
- Cross-page shared state without documentation

---

# 4. Side Effect Governance

All side effects must:

- Be isolated in hooks
- Declare full dependency arrays
- Avoid hidden async race conditions
- Implement cancellation where needed

Prohibited:

- Business logic inside useEffect without separation
- Network calls inside UI layer
- Uncontrolled polling

---

# 5. API Interaction Governance

Each API must define:

- Request type
- Response type
- Error type
- Retry policy
- Timeout strategy

Must evaluate:

- Is it idempotent?
- Is caching needed?
- Is debouncing needed?

---

# 6. Performance Governance

Mandatory evaluation:

- Render frequency
- Dependency stability
- Memoization necessity
- Expensive calculation isolation

Prohibited:

- Premature optimization
- Blind memoization without profiling

---

# 7. Dependency Governance

Every new dependency must document:

- Why it is needed
- Bundle size impact
- Maintenance status
- Alternative evaluation

---

# 8. CI Governance

CI must enforce:

- Type checking
- ESLint
- Build success
- Unit tests (if applicable)

No AI-generated code may bypass CI validation.

---

# 9. Refactoring Governance

When modifying:

- Shared components
- Base hooks
- API layer
- Type definitions

Must trigger risk reassessment in PR Gate phase.

---

# 10. Governance Enforcement Mapping

| Phase | Must Reference Section |
|-------|------------------------|
| Phase 1 | Architecture + Domain |
| Phase 2 | Type + State + Side Effects |
| Phase 3 | API + Performance |
| Phase 4 | Dependency + Refactoring |

---

# Summary

This document defines the **non-negotiable baseline**.

All Planning, Development, Self-Test, and PR Gate phases must comply with these rules.

Deviation requires explicit justification.