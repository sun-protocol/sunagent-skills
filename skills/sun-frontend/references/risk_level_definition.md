# Risk Level Definition Model

Version: v1.0.0  
Purpose: Provide structured risk evaluation criteria for PR Gate phase.  
Scope: React + TypeScript enterprise frontend projects.

---

# 1. Risk Evaluation Dimensions

Risk level must be evaluated across the following dimensions:

1. Impact Radius
2. State Modification Scope
3. API Contract Changes
4. Shared Component Impact
5. Base Hook Modification
6. Dependency Changes
7. Rollback Complexity

Each dimension contributes to the final risk classification.

---

# 2. Impact Radius

Defines how widely the change affects the system.

| Level | Description |
|-------|------------|
| Local | Single component/page only |
| Module | Multiple components within one module |
| Cross-Module | Impacts multiple feature modules |
| Global | Impacts shared infrastructure or layout |

---

# 3. State Modification Scope

| Type | Risk Indicator |
|------|----------------|
| Local state only | Low |
| Introduces new shared state | Medium |
| Modifies existing shared state | High |
| Refactors global state architecture | Critical |

---

# 4. API Contract Changes

Evaluate:

- Request shape change
- Response shape change
- Error model change
- Timeout/retry behavior change

| Scenario | Risk |
|----------|------|
| No contract change | Low |
| Adds optional field | Medium |
| Removes/changes field | High |
| Breaks backward compatibility | Critical |

---

# 5. Shared Component Impact

If modification involves:

- Design system components
- Base layout
- Common form components
- Shared table components

Then risk automatically escalates by one level.

---

# 6. Base Hook Modification

Hooks considered “base hooks”:

- useRequest wrappers
- useAuth
- usePermission
- useGlobalStore
- Custom abstraction over API layer

Modification rule:

| Change Type | Risk |
|-------------|------|
| Internal bug fix | Medium |
| Signature change | High |
| Behavioral change | Critical |

---

# 7. Dependency Changes

Evaluate:

- New third-party library introduced
- Major version upgrade
- Removal of dependency
- Peer dependency impact

If bundle size increases significantly, risk escalates.

---

# 8. Rollback Complexity

Assess:

- Can revert via git revert?
- Requires backend coordination?
- Requires data migration?
- Requires cache invalidation?

If rollback requires coordinated multi-system effort → risk escalates.

---

# 9. Risk Level Classification

Final risk must be classified as:

## Low

- Local impact
- No shared state modification
- No API contract changes
- Easy rollback

## Medium

- Module-level impact
- Adds shared state
- Minor API adjustments
- Controlled rollback

## High

- Cross-module impact
- Shared component modification
- API contract modification
- Complex rollback

## Critical

- Global architectural change
- Base hook refactor
- Breaking API change
- Multi-system coordination required

---

# 10. PR Gate Output Mapping

PR Gate must explicitly state:

- Risk Level
- Triggered Dimensions
- Justification
- Recommended Release Strategy:
  - Direct release
  - Gradual rollout
  - Feature flag
  - Shadow release

---

# 11. Risk Escalation Rule

If two or more dimensions are classified as High,
final risk must not be lower than High.

If any dimension is Critical,
final risk = Critical.

---

# Summary

Risk evaluation must be:

- Explicit
- Justified
- Traceable
- Documented in PR Gate phase

No subjective “looks safe” judgment is allowed.