---
name: DeFi Smart Contract Audit
description: A rigorous, token-efficient DeFi smart contract audit process with specialized checks for DEX, AMM, Lending, vaults, routers, staking, and token interactions. Produces only the final audit report artifact.
version: "2.10.0"
updated: "2026-06"
---

# DeFi Smart Contract Audit Skill

This skill performs a high-precision DeFi smart contract audit while minimizing output-token usage. It keeps phase work as internal working notes, omits low-value process detail, and writes only the final audit report.

## Resources

| Resource | Path | When to Load |
|----------|------|--------------|
| Audit Report Template | `resources/audit_report_template.md` | Before writing the final report |
| Vulnerability Patterns | `references/VULNERABILITY_PATTERNS.md` | During vulnerability discovery |
| DeFi Checklist | `references/DEFI_CHECKLIST.md` | During vulnerability discovery |

## Output Policy

Only one artifact may be written:

```text
.audit/Audit-Report.md
```

Do not create `.audit/blueprints/` files. Do not output full intermediate phase reports in the conversation. During the audit, keep concise internal notes for architecture, function inventory, data flows, findings, and verification. In the conversation, provide only short progress updates unless the user explicitly asks for intermediate details.

## Omission Policy

The following content is more expensive than useful by default. Omit it unless the user explicitly asks for it:

- Full function inventories.
- Full checklist pass/fail tables.
- Full vulnerability-pattern pass/fail tables.
- Phase statistics, verification-category counts, and code re-reading efficiency metrics.
- Raw Slither, Semgrep, Mythril, Forge, Hardhat, or compiler output.
- Full tool JSON output.
- Detailed false-positive analysis.
- Long lists of low-risk or informational issues.

Preserve only the parts needed to support confirmed, security-relevant findings:

- High-risk contracts, functions, and flows.
- Code references for confirmed findings.
- Concise coverage summaries.
- Tool status and high-signal confirmed tool findings.
- Residual scope limitations.

## Workflow Overview

1. Phase 0: Architecture and protocol classification.
2. Phase 1: Optional automated scan.
3. Phase 2: Contract understanding.
4. Phase 3: Vulnerability discovery.
5. Phase 4: Verification and impact assessment.
6. Final: Write `.audit/Audit-Report.md`.

Execute phases sequentially, but do not persist phase outputs. Later phases should reference the internal notes created earlier in the same audit run.

## Phase 0: Architecture and Protocol Classification

Goal: understand what is being audited before looking for vulnerabilities.

Actions:

- Inspect repository structure, README/specs if present, and main contracts.
- Classify protocol type: DEX/AMM, Lending, Yield/Vault, Stablecoin/PSM, Bridge/Router, Staking, Token, Governance, or Other.
- Identify key contracts, external integrations, privileged roles, upgradeability/proxy patterns, and money flow.
- Note documentation mismatches or missing specs.

Internal notes to preserve for later phases:

- Protocol type and chain.
- Key contracts and responsibilities.
- External integrations and trust assumptions.
- Roles and privileged operations.
- Money flow and asset custody points.
- Documentation gaps.

## Phase 1: Automated Scan (Optional)

Goal: supplement manual review with tool findings when tools are available and cheap enough to run.

Run only tools that fit the project and environment. Skip this phase if tools are unavailable, dependency installation would be expensive, or the user requests a lightweight audit.

Useful commands:

```bash
slither . --exclude-dependencies
slither . --json slither-report.json --exclude-dependencies
slither . --detect reentrancy-eth,reentrancy-no-eth,arbitrary-send,controlled-delegatecall,suicidal,unprotected-upgrade
semgrep --config "p/smart-contracts" .
forge build
forge test
npx hardhat compile
npx hardhat test
```

Use Mythril only when specifically useful for a narrow contract or finding. Avoid full-project Mythril by default because symbolic execution can be slow and noisy.

Internal notes to preserve:

- Tools run or skipped, with reason.
- Compilation/test status.
- High-signal tool findings.
- Findings that need manual confirmation.

## Phase 2: Contract Understanding

Goal: build enough understanding to audit logic, not just match patterns.

Read contracts top to bottom, prioritizing in-scope and asset-touching contracts. For large projects, first build a contract map, then deep-read critical paths.

For each high-risk function, capture:

- Visibility, modifiers, and caller assumptions.
- Purpose and role in the protocol.
- Parameter semantics and validation.
- State changes and asset movements.
- External calls, callbacks, token transfers, and oracle reads.
- Events, errors, and return values.
- Related internal functions and call flow.
- Critical assumptions and validation gaps.

Also capture:

- Money flows: entry points, accounting, exit points, stuck-fund risks.
- State transitions and invariants.
- Function dependency graph and critical paths.
- Cross-function consistency for similar actions.

High-risk functions include functions that touch assets, accounting, authorization, upgrades, oracle data, signatures, callbacks, external calls, token approvals, swaps, mint/burn logic, liquidations, or reward distribution. For low-risk helpers, record only unusual assumptions or issues. Do not write or print the full function inventory unless the user asks.

## Phase 3: Vulnerability Discovery

Goal: find logic errors, state inconsistencies, and DeFi-specific vulnerabilities.

At the start of this phase, load only the relevant sections of:

- `references/DEFI_CHECKLIST.md`
- `references/VULNERABILITY_PATTERNS.md`

Use two complementary approaches:

### Function-Based Analysis

For each in-scope function, re-read the code and check:

- Parameter validation gaps.
- Access control and authorization.
- Reentrancy and callback surfaces.
- Arithmetic, precision, rounding, unsafe casts.
- State update ordering.
- External call safety.
- Return value and token transfer handling.
- Edge cases: zero values, max values, empty arrays, `address(0)`, fee-on-transfer tokens, rebasing tokens.

### Flow-Based Analysis

Trace complete protocol flows and check cross-function behavior:

- Deposit/withdraw.
- Swap/exchange.
- Add/remove liquidity.
- Borrow/repay/liquidate.
- Stake/unstake/claim.
- Admin and upgrade flows.
- Permit/meta-transaction flows.
- Multicall, router, aggregator, callback, and hook flows.

For each issue candidate, keep compact internal evidence:

- Finding ID and title.
- Severity candidate.
- Location and line references.
- Vulnerable code summary.
- Exploit path.
- Impact.
- Recommendation.
- Verification status: complete evidence, needs targeted review, or uncertain.

Do not output the full Phase 3 report. These notes feed Phase 4 and the final report.

For checklist and vulnerability-pattern coverage, track only:

- Categories reviewed.
- Categories with confirmed issues.
- Categories intentionally skipped as out of scope.

Do not produce item-by-item pass/fail tables unless requested.

## Phase 4: Verification and Impact Assessment

Goal: reduce false positives and assign defensible severity.

For each candidate finding:

- Define the violated safety invariant.
- Confirm exploit feasibility.
- Check whether assumptions are realistic.
- Assess maximum extractable value or protocol/user impact.
- Check whether flash loans, oracle manipulation, reentrancy, or MEV amplify the issue.
- Re-read code only when the candidate lacks enough evidence or may be mitigated elsewhere.
- Mark final status: Confirmed, Disputed, False Positive, or Needs More Info.

Only confirmed security-relevant findings should appear as main findings in the final report. Disputed or false-positive items should be omitted unless they materially affect residual risk. Low and informational issues should be compressed into a short table when they do not justify full finding treatment.

## Severity Classification

| Severity | Impact | Likelihood | Examples |
|----------|--------|------------|----------|
| Critical | Direct fund loss, protocol insolvency | High | Drainable reentrancy, unprotected initialization, arbitrary external call draining approvals |
| High | Significant damage or partial fund loss | Medium to high | Access control bypass, oracle manipulation, missing output validation |
| Medium | Conditional impact requiring setup | Medium | Precision loss, timing edge cases, callback origin mistakes |
| Low | Minor security or operational issue | Low | Missing events, weak validation with low impact, hardcoded operational limits |
| Informational | Best practice or maintainability | N/A | Documentation gaps, non-security code quality observations |

## Final Report Generation

Before writing the report:

- Load `resources/audit_report_template.md`.
- Use internal Phase 0-4 notes as sources.
- Include only concise, verified content.
- Avoid detailed phase logs, function inventories, verification-category statistics, and analysis process narration.
- Exclude raw tool output and unconfirmed tool findings.

Write the final report to:

```text
.audit/Audit-Report.md
```

The report must include:

- Executive summary with scope, severity counts, and key risk summary.
- Scope and methodology, briefly.
- Confirmed findings grouped by severity.
- For each finding: title, severity, affected files/functions, description, impact, exploit scenario, recommendation, and relevant code reference.
- Optional compact table for low/informational notes.
- Appendix with concise checklist/pattern coverage summary when useful, using category-level coverage only.
- Limitations and assumptions.

After saving the report, summarize to the user:

- Path written.
- Finding counts by severity.
- Tests/tools run or skipped.
- Any residual risks or incomplete scope.

## Core Rules

- Keep phase work internal and concise.
- Do not write intermediate audit files.
- Do not paste full intermediate phase outputs into chat.
- Do not include full inventories, full checklist tables, raw tool output, or verbose verification logs by default.
- Re-read code with a different focus in each phase.
- Understand before auditing.
- Prioritize asset custody, privileged actions, external calls, oracles, accounting, and cross-function flows.
- Use automated tools as support, not authority.
- Prefer narrow, evidence-backed findings over speculative lists.
- If the project is too large for one pass, audit the highest-risk contracts first and state the remaining scope in the final report.
