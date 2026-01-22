---
name: DeFi Smart Contract Audit
description: A rigorous, adversarial DeFi smart contract audit process with specialized checks for DEX, AMM, Lending, and token interactions.
version: "2.8.0"
updated: "2026-01"
---

# DeFi Smart Contract Audit Skill (Advanced)

This skill executes a high-precision security audit using a "Team of Rivals" approach, **specialized for DeFi protocols**.

## 📂 Skill Resources

| Resource | Path | Description |
|----------|------|-------------|
| **Audit Report Template** | `resources/audit_report_template.md` | Template for final comprehensive audit report (`.audit/Audit-Report.md`) |
| **Vulnerability Patterns** | `references/VULNERABILITY_PATTERNS.md` | Solidity vulnerability code examples - Use in Phase 3 (Vulnerability Discovery) |
| **DeFi Checklist** | `references/DEFI_CHECKLIST.md` | Protocol-specific audit checklists - Use in Phase 3 Step 3.3 (DeFi-Specific Vulnerability Scan) |

## 🚀 Workflow Overview

1.  **Phase 0 (Architect)**: Map architecture, identify DeFi protocol type, and verify documentation.
2.  **Phase 1 (Automated Scan)**: Run static analysis tools (Slither, Mythril) to catch low-hanging fruit.
3.  **Phase 2 (Contract Understanding)**: ⚠️ **NEW** Deep understanding of contract architecture, function purposes, data flows, and parameter semantics.
4.  **Phase 3 (Senior Auditor)**:
    *   Step 3.1: Storage Layout & Global Scan.
    *   Step 3.2: **Flow-Based** Function Analysis.
    *   Step 3.3: **DeFi-Specific Vulnerability Scan** (CRITICAL).
    *   Step 3.4: **Solidity Gotchas Checklist**.
5.  **Phase 4 (Defense)**: Verify findings using **Invariants** and assess real-world impact.

---

## 🛠️ Instructions

> ⚠️ **CRITICAL EXECUTION ORDER**: You MUST execute phases **SEQUENTIALLY**, one at a time. Each phase must be completed, output in conversation, and **SAVED BY AI using `write` tool** to a separate file before starting the next phase.
>
> **⚠️ MANDATORY FILE-BASED WORKFLOW:**
>
> **⚠️ FILE STRUCTURE**: All audit files must be saved in the `.audit/` directory with the following structure:
> ```
> .audit/
> ├── blueprints/
> │   ├── 0_Setup.md
> │   ├── 1_Automated_Security_Scan.md
> │   ├── 2_Understanding.md
> │   ├── 3_Vulnerability_Scan.md
> │   └── 4_Defense_Verification.md
> └── Audit-Report.md (final report)
> ```
>
> **Phase 0:**
> 1. Read necessary files (README, main contracts) for architecture understanding
> 2. Complete Phase 0 analysis
> 3. **MANDATORY**: Output complete Phase 0 content in conversation (formatted as markdown)
> 4. **MANDATORY**: **SAVE** this content to `.audit/blueprints/0_Setup.md` using `write` tool immediately after outputting
> 5. **VERIFICATION**: When starting Phase 1, **MUST** use `read_file` to read `.audit/blueprints/0_Setup.md` - verify it exists and contains Phase 0 results. If file doesn't exist or is empty, STOP and re-run Phase 0.
> 6. **DO NOT** proceed to Phase 1 until Phase 0 file is verified to exist and contains content
>
> **Phase 1:**
> 1. **MANDATORY**: **FIRST STEP** - Use `read_file` tool to read `.audit/blueprints/0_Setup.md` - verify it exists and contains Phase 0 results. If file doesn't exist or is empty, STOP and re-run Phase 0.
> 2. **MANDATORY**: Re-read relevant code files with Phase 1 focus (automated scan patterns, security patterns)
> 3. Complete Phase 1 analysis (if tools unavailable, perform manual pattern-based analysis)
> 4. **MANDATORY**: Output complete Phase 1 content in conversation (formatted as markdown)
> 5. **MANDATORY**: **SAVE** this content to `.audit/blueprints/1_Automated_Security_Scan.md` using `write` tool immediately after outputting
> 6. **VERIFICATION**: When starting Phase 2, **MUST** use `read_file` to read `.audit/blueprints/0_Setup.md` and `.audit/blueprints/1_Automated_Security_Scan.md` - verify both exist and contain content
> 7. **DO NOT** proceed to Phase 2 until Phase 1 file is verified to exist and contains content
>
> **Phase 2:**
> 1. **MANDATORY**: **FIRST STEP** - Use `read_file` tool to read `.audit/blueprints/0_Setup.md` and `.audit/blueprints/1_Automated_Security_Scan.md` - verify both exist and contain results. If files don't exist or are empty, STOP and re-run previous phases.
> 2. **MANDATORY**: Re-read ALL contract files from top to bottom with Phase 2 focus (deep understanding, function inventory)
> 3. Complete Phase 2 analysis (this is the most comprehensive phase - take your time)
> 4. **MANDATORY**: Output complete Phase 2 content in conversation (formatted as markdown)
> 5. **MANDATORY**: **SAVE** this content to `.audit/blueprints/2_Understanding.md` using `write` tool immediately after outputting
> 6. **VERIFICATION**: When starting Phase 3, **MUST** use `read_file` to read `.audit/blueprints/0_Setup.md`, `.audit/blueprints/1_Automated_Security_Scan.md`, and `.audit/blueprints/2_Understanding.md` - verify all exist and contain comprehensive function inventory
> 7. **DO NOT** proceed to Phase 3 until Phase 2 file is verified to exist and contains function inventory
>
> **Phase 3:**
> 1. **MANDATORY**: **FIRST STEP** - Use `read_file` tool to read `.audit/blueprints/0_Setup.md`, `.audit/blueprints/1_Automated_Security_Scan.md`, and `.audit/blueprints/2_Understanding.md` - verify all exist and contain results. If files don't exist or are empty, STOP and re-run previous phases.
> 2. **MANDATORY**: Reference Phase 2 function inventory and parameter semantics when analyzing vulnerabilities - explicitly cite Phase 2 findings in your analysis
> 3. **MANDATORY**: Re-read relevant code sections with Phase 3 focus (vulnerability discovery based on Phase 2 understanding)
> 4. Complete Phase 3 analysis (check ALL items in DeFi-specific vulnerability checklist)
> 5. **MANDATORY**: Output complete Phase 3 content in conversation (formatted as markdown with all vulnerability findings)
> 6. **MANDATORY**: **SAVE** this content to `.audit/blueprints/3_Vulnerability_Scan.md` using `write` tool immediately after outputting
> 7. **VERIFICATION**: When starting Phase 4, **MUST** use `read_file` to read all previous blueprint files (`.audit/blueprints/0_Setup.md` through `.audit/blueprints/3_Vulnerability_Scan.md`) - verify all exist and contain vulnerability findings
> 8. **DO NOT** proceed to Phase 4 until Phase 3 file is verified to exist and contains findings
>
> **Phase 4:**
> 1. **MANDATORY**: Use `read_file` tool to read all previous blueprint files (`.audit/blueprints/0_Setup.md` through `.audit/blueprints/3_Vulnerability_Scan.md`) - verify all exist and contain results. If files don't exist or are empty, STOP and ask user to complete previous phases first.
> 2. **MANDATORY**: For EACH finding from Phase 3, re-read relevant code sections and perform verification
> 3. Complete Phase 4 analysis (verification and impact assessment for each Phase 3 finding)
> 4. **MANDATORY**: Output complete Phase 4 content in conversation (formatted as markdown with verification results for each finding)
> 5. **MANDATORY**: Instruct user to save this content to `.audit/blueprints/4_Defense_Verification.md`
> 6. **VERIFICATION**: When generating final report, use `read_file` to verify Phase 4 file exists and contains verification results
>
> **Final Report:**
> 1. **MANDATORY**: Use `read_file` tool to read `.audit/blueprints/3_Vulnerability_Scan.md` and `.audit/blueprints/4_Defense_Verification.md` (primary sources)
> 2. **OPTIONAL**: Reference `.audit/blueprints/0_Setup.md`, `.audit/blueprints/1_Automated_Security_Scan.md`, and `.audit/blueprints/2_Understanding.md` for context
> 3. **FOCUS**: The final report should center around Phase 3 (Vulnerability Scan) and Phase 4 (Defense & Verification) comprehensive results
> 4. **Structure**: Include executive summary, findings from Phase 3 with Phase 4 verification, severity classification, and recommendations
> 5. **Output**: Output final report in conversation (formatted as markdown)
> 6. **Instruct User**: "Please save this content to `.audit/Audit-Report.md`. This is the final comprehensive audit report focusing on vulnerabilities and their verification."
>
> **DO NOT:**
> - ❌ Read all files at once and analyze everything together
> - ❌ Output all phases in a single execution
> - ❌ Skip ahead to later phases before completing earlier ones
> - ❌ Skip reading previous phase files when starting a new phase
> - ❌ Skip re-reading code files for each phase (each phase has different focus)
> - ❌ Combine multiple phases into one output
>
> **DO:**
> - ✅ Complete one phase fully before starting the next
> - ✅ Output each phase's complete content in conversation after completing it
> - ✅ **SAVE each phase to a separate file using `write` tool immediately after outputting**
> - ✅ **ALWAYS read previous phase files using `read_file` tool as the FIRST STEP when starting a new phase**
> - ✅ Re-read relevant code files for each phase (each phase has different focus)
> - ✅ Reference previous phase findings explicitly in your analysis
> - ✅ Use findings from earlier phases to inform later phases

---

### Phase 0: Architecture & Protocol Classification

> ⚠️ **MANDATORY FILE OPERATIONS**:
> - At the START: Read `README.md` (if exists) and main contract files to understand the project
> - At the END: Output complete Phase 0 content in conversation (following the Phase 0 Output Format below), then **SAVE** to `.audit/blueprints/0_Setup.md` using `write` tool
> - **VERIFICATION**: When starting Phase 1, **MUST** read Phase 0 file using `read_file` tool as the FIRST STEP
> - **DO NOT** proceed to Phase 1 until Phase 0 file is verified to exist and contains content

1.  **Initialize**: Read project structure and main contract files.
2.  **Identify Protocol Type**: Classify the DeFi protocol:
    *   `DEX/AMM` - Token swaps, liquidity pools (Uniswap, Curve, etc.)
    *   `Lending` - Borrow/Lend protocols (Aave, Compound, etc.)
    *   `Yield/Vault` - Yield aggregators, vaults (Yearn, etc.)
    *   `Stablecoin/PSM` - Stablecoin mechanisms, PSM modules
    *   `Bridge/Router` - Cross-chain or multi-protocol routers
    *   `Staking` - Token staking, reward distribution
3.  **Doc Check**: Read `README.md` or specs (if exists). Compare against the code outline.
    *   **If README is empty or generic**: Mark "Documentation vs Code Discrepancies" as `N/A - No meaningful spec provided` and proceed.
4.  **Action**: Document all Phase 0 findings, specifically noting:
    - Protocol type classification
    - Architecture overview
    - Key contracts and their purposes
    - Money flow diagram
    - Roles & access control
    - External integrations
    - Documentation mismatches

**Phase 0 Output Format**:
```markdown
# Phase 0: Architecture & Protocol Classification

## Protocol Classification
- **Type:** [DEX/AMM | Lending | Yield/Vault | Stablecoin/PSM | Bridge/Router | Staking]
- **Blockchain:** [Ethereum | TRON | BSC | etc.]
- **Solidity Version:** [version range]

## Key Contracts
| Contract | LOC | Purpose |
|----------|-----|---------|
| [Contract Name] | [lines] | [Purpose] |

## Money Flow Architecture
[Diagram or narrative description]

## Roles & Access Control
| Role | Storage | Permissions | Transfer Method |
|------|---------|-------------|-----------------|
| [Role] | [var] | [permissions] | [method] |

## External Integrations
| Integration | Interface | Risk Level | Notes |
|-------------|-----------|------------|-------|
| [Integration] | [Interface] | [Level] | [Notes] |

## Documentation vs Code Discrepancies
[Findings or N/A]
```

5.  **MANDATORY OUTPUT**: Output complete Phase 0 content in conversation (formatted as markdown following the format above). Then **IMMEDIATELY SAVE** this content to `.audit/blueprints/0_Setup.md` using `write` tool. After saving, proceed to Phase 1.

---

### Phase 1: Automated Security Scan
**Goal**: Use static analysis tools to find common vulnerabilities before manual review.

> ⚠️ **MANDATORY FILE OPERATIONS**:
> - At the START: **FIRST STEP** - **MUST** use `read_file` tool to read `.audit/blueprints/0_Setup.md` - verify it exists and contains Phase 0 results. If file doesn't exist or is empty, STOP and re-run Phase 0.
> - During analysis: Re-read relevant code files with Phase 1 focus (security patterns, compilation issues)
> - At the END: Output complete Phase 1 content in conversation (following the Phase 1 Output Format below), then **SAVE** to `.audit/blueprints/1_Automated_Security_Scan.md` using `write` tool
> - **VERIFICATION**: When starting Phase 2, **MUST** read Phase 1 file using `read_file` tool as the FIRST STEP
> - **DO NOT** proceed to Phase 2 until Phase 1 file is verified to exist and contains content

> **Reference**: This phase uses methodology from the `smart-contract-security` skill.

#### Step 1: Static Analysis Tools

> ⚠️ **IMPORTANT**: If automated tools (Slither, Mythril) are not available or cannot be run, perform **manual pattern-based analysis** instead. The goal is to identify common vulnerability patterns, not necessarily to run the tools.

**Option A: If tools are available** (user can run them):
- **Slither** (Fast vulnerability detection):
  ```bash
  # Basic scan
  slither . --exclude-dependencies
  
  # JSON output for parsing
  slither . --json slither-report.json --exclude-dependencies
  
  # Focus on high-impact detectors only
  slither . --detect reentrancy-eth,reentrancy-no-eth,arbitrary-send,controlled-delegatecall,suicidal,unprotected-upgrade
  ```
- **Mythril** (Symbolic execution - deeper analysis):
  ```bash
  # Single contract analysis
  myth analyze contracts/Contract.sol --solc-json mythril.config.json
  
  # Full project
  myth analyze . --solc-json mythril.config.json
  ```
- **Semgrep** (Custom rules for smart contracts):
  ```bash
  semgrep --config "p/smart-contracts" .
  ```

**Option B: Manual pattern-based analysis** (if tools unavailable):
- Manually check for patterns listed in Step 3 (Triage Automated Findings)
- Review code for common vulnerability patterns (reentrancy, access control, etc.)
- Document findings as if they came from automated tools (with `[MANUAL]` tag instead of `[AUTO]`)

#### Step 2: Pre-Audit Checklist
Before proceeding to manual review, verify:

- [ ] Code compiles without warnings (`forge build` or `npx hardhat compile`)
- [ ] Tests pass with good coverage (`forge test` or `npx hardhat test`)
- [ ] No critical Slither findings unaddressed
- [ ] Dependencies reviewed (check for known vulnerabilities)

#### Step 3: Triage Automated Findings

| Slither Detector | Severity | Action |
|-----------------|----------|--------|
| `reentrancy-eth` | Critical | Manual review required |
| `arbitrary-send` | Critical | Manual review required |
| `suicidal` | Critical | Manual review required |
| `unprotected-upgrade` | High | Check if intentional |
| `controlled-delegatecall` | High | Manual review required |
| `unchecked-transfer` | Medium | Verify SafeERC20 usage |
| `reentrancy-benign` | Low | Usually false positive |
| `naming-convention` | Info | Ignore unless egregious |

**Action**: Document all Phase 1 findings, including:
- Pre-audit checklist results (with checkboxes: [x] or [ ])
- Automated scan status (whether tools were run, or manual analysis performed)
- Key security patterns identified (ReentrancyGuard, SafeMath, access control, etc.)
- Any automated tool findings (with `[AUTO]` tag) or manual pattern findings (with `[MANUAL]` tag)

**Phase 1 Output Format**:
```markdown
# Phase 1: Automated Security Scan

## Pre-Audit Checklist
- [x] Code compiles without warnings
- [ ] Tests pass with good coverage
- [x] No critical findings unaddressed
- [x] Dependencies reviewed

## Automated Scan Status
[Status: Tools run / Manual analysis / Not available]

## Key Security Patterns Identified
[Table or list of patterns found]

## Automated/Manual Findings
[Any findings from tools or manual pattern matching]
```

**MANDATORY OUTPUT**: Output complete Phase 1 content in conversation (formatted as markdown following the format above). Then **IMMEDIATELY SAVE** this content to `.audit/blueprints/1_Automated_Security_Scan.md` using `write` tool. After saving, proceed to Phase 2.

---

### Phase 2: Contract Understanding (MANDATORY)

**Goal**: Before diving into vulnerability discovery, you MUST achieve a deep, comprehensive understanding of the contract's architecture, design intent, and implementation details.

> ⚠️ **MANDATORY FILE OPERATIONS**:
> - At the START: **FIRST STEP** - **MUST** use `read_file` tool to read `.audit/blueprints/0_Setup.md` and `.audit/blueprints/1_Automated_Security_Scan.md` - verify both exist and contain results. If files don't exist or are empty, STOP and re-run previous phases.
> - During analysis: **MANDATORY** Re-read ALL contract files from top to bottom (this is the most comprehensive code reading phase)
> - At the END: Output complete Phase 2 content in conversation (with ALL required sections: architecture, function inventory, data flows, dependency graph, parameter semantics, consistency check, and understanding summary), then **SAVE** to `.audit/blueprints/2_Understanding.md` using `write` tool
> - **VERIFICATION**: When starting Phase 3, **MUST** read Phase 2 file using `read_file` tool as the FIRST STEP - verify it exists and contains comprehensive function inventory
> - **DO NOT** proceed to Phase 3 until Phase 2 file is verified to exist and contains function inventory

> ⚠️ **CRITICAL**: This phase is MANDATORY. Do NOT proceed to Phase 3 until you have completed ALL steps below and output your understanding.
>
> ⚠️ **CODE READING**: This phase requires the MOST comprehensive code reading. You MUST read every function, every modifier, every state variable. Take your time to truly understand the contract.

#### Step 2.1: Contract Architecture Understanding

1. **Read the entire contract from top to bottom** - Do NOT skip any function or modifier.
2. **Identify the contract's primary purpose**: What problem does it solve? What is its core functionality?
3. **Map the contract hierarchy**:
   - What contracts does it inherit from?
   - What libraries does it use?
   - What interfaces does it implement?
4. **Identify design patterns**:
   - Is it using proxy pattern? Upgradeable?
   - Is it using factory pattern?
   - Is it using router/aggregator pattern?
   - Any other architectural patterns?

**Output**: Document in Phase 2 output under "Phase 2: Contract Understanding" section.

#### Step 2.2: Complete Function Inventory & Documentation

**For EVERY function in the contract**, create a detailed entry:

```markdown
### Function: `functionName`

**Visibility**: `public` / `external` / `internal` / `private`
**Modifiers**: `onlyOwner`, `nonReentrant`, etc.
**Purpose**: What does this function do? What is its role in the contract?
**Input Parameters**:
  - `param1` (type): What does this represent? What are valid values? What are edge cases?
  - `param2` (type): [same analysis]
**Output Parameters**:
  - `return1` (type): What does this represent? What are possible values?
**State Changes**: What state variables does this function modify?
**External Calls**: What external contracts does this call? Why?
**Events Emitted**: What events? What information do they convey?
**Error Conditions**: When does this function revert? What are the error messages?
**Call Flow**: Who can call this? What functions call this?
**Dependencies**: What other functions does this depend on?
```

**Example Template** (adapt to your specific protocol type):

```markdown
### Function: `[functionName]`

**Visibility**: `external payable` / `public` / `internal` / `private`
**Modifiers**: `nonReentrant`, `onlyOwner`, etc.
**Purpose**: [What does this function do? What is its role in the protocol?]

**Input Parameters**:
  - `param1` (type): [What does this represent? What are valid values? What are edge cases?]
    - **Semantics**: [Business logic meaning]
    - **Valid values**: [Acceptable ranges/values]
    - **Edge cases**: [Boundary conditions, special values like address(0)]
    - **Validation**: [Where is it validated? Is validation sufficient?]
  - `param2` (type): [same analysis for each parameter]

**Output Parameters**:
  - `return1` (type): [What does this represent? What are possible values?]
    - **Semantics**: [What does the return value mean in business logic?]

**State Changes**: 
  - [What state variables does this modify?]
  - [What balances/allowances change?]

**External Calls**:
  - [What external contracts are called? Why?]
  - [Are these calls safe?]

**Events Emitted**: 
  - `EventName`: [What information does this convey?]

**Error Conditions**:
  - `ERROR_NAME`: [When does this revert? What are the conditions?]

**Call Flow**:
  - [Who can call this? Entry point or internal?]
  - [What functions call this?]

**Dependencies**:
  - [What other functions does this depend on?]
  - [What libraries/helpers are used?]

**Critical Assumptions**:
  - [What assumptions does this function make?]
  - [Are these assumptions validated?]
```

**Action**: Create this documentation for ALL functions (public, external, internal, private).

#### Step 2.3: Data Flow Analysis

1. **Trace money flows**:
   - Where does money enter the contract?
   - How does it flow through the contract?
   - Where does it exit?
   - Are there any places where money can get stuck?

2. **Trace state transitions**:
   - What are the key state variables?
   - How do they change over time?
   - What are the valid state transitions?
   - Are there invalid states that can be reached?

3. **Trace parameter flows**:
   - How do user inputs flow through the contract?
   - Are inputs validated at each step?
   - Are there places where inputs are used without validation?
   - Do parameters maintain their semantic meaning throughout?

**Output**: Create a data flow diagram or narrative in the Phase 2 output.

#### Step 2.4: Function Dependency Graph

1. **Map function call relationships**:
   - Which functions call which other functions?
   - What is the call depth?
   - Are there circular dependencies?
   - Are there unreachable functions?

2. **Identify function groups**:
   - Entry point functions (called by users)
   - Internal routing functions (called by entry points)
   - Helper functions (called by multiple functions)
   - Admin functions (called by owner/admin)

3. **Identify critical paths**:
   - What are the most common execution paths?
   - What are the edge case paths?
   - What paths handle native tokens (if applicable)?
   - What paths handle multi-step operations (if applicable)?

**Output**: Create a function dependency graph or table in Phase 2 output.

#### Step 2.5: Parameter Semantics & Validation Analysis

**For each function, analyze**:

1. **Input Parameter Semantics**:
   - What does each parameter represent in the business logic?
   - What are the implicit assumptions about each parameter?
   - Example: For swap functions, `address(0)` might mean "native token" - does the code handle this correctly?
   - Example: For lending protocols, `collateralFactor` might be a percentage - is it validated to be within bounds?

2. **Parameter Validation**:
   - Where are parameters validated? (Entry point? Internal functions? Both?)
   - Are validations consistent across different code paths?
   - Are there parameters used without validation?
   - Example: If a function assumes a specific format (e.g., native token must be wrapped first), is this validated BEFORE the operation?

3. **Parameter Transformation**:
   - How are parameters transformed as they flow through the contract?
   - Do transformations preserve semantic meaning?
   - Example: Arrays might be split into slices - are validations applied to the slice?
   - Example: Amounts might be converted between different units - is precision preserved?

4. **Output Parameter Semantics**:
   - What do return values represent?
   - Are return values always meaningful?
   - Example: If a function returns an array, are all elements populated or only some? Is this intentional?

**Output**: Document parameter semantics and validation gaps in Phase 2 output.

#### Step 2.6: Cross-Function Consistency Check

1. **Compare similar functions**:
   - Do functions that perform similar operations have consistent validation?
   - Do they handle edge cases the same way?
   - Example: If there are multiple swap functions (e.g., swapExactInput, swapExactOutput), do they validate inputs consistently?
   - Example: If there are multiple deposit functions (e.g., depositETH, depositToken), do they handle native tokens consistently?

2. **Check for inconsistencies**:
   - Are validation rules applied consistently across similar functions?
   - Are error messages consistent?
   - Are parameter assumptions consistent?
   - Example: If one function validates a parameter but another similar function doesn't, this is an inconsistency.

**Output**: List any inconsistencies found in Phase 2 output.

#### Step 2.7: Create Understanding Summary

**Before completing Phase 2, create a summary**:

```markdown
## Contract Understanding Summary

### Core Functionality
[One paragraph describing what the contract does]

### Key Design Decisions
- [Decision 1 and rationale]
- [Decision 2 and rationale]

### Critical Assumptions
- [Assumption 1: e.g., "Native token must be wrapped before certain operations"]
- [Assumption 2: e.g., "All input addresses must exist in registry"]
- [Assumption 3: Protocol-specific assumptions based on your analysis]

### Known Validation Gaps
- [Gap 1: e.g., "Deadline not validated at entry point"]
- [Gap 2: e.g., "Input format not validated before transformation"]
- [Gap 3: Protocol-specific gaps based on your analysis]

### Areas Requiring Deep Audit
- [Area 1: e.g., "Input validation in all entry functions"]
- [Area 2: e.g., "Native token handling across all functions"]
- [Area 3: Protocol-specific areas based on your analysis]
```

**Action**: Complete ALL steps above and include everything in Phase 2 output. This understanding will guide your vulnerability discovery in Phase 3.

**MANDATORY OUTPUT**: Output complete Phase 2 content in conversation (formatted as markdown with all sections: architecture, function inventory, data flows, dependency graph, parameter semantics, consistency check, and understanding summary). Then **IMMEDIATELY SAVE** this content to `.audit/blueprints/2_Understanding.md` using `write` tool. After saving, proceed to Phase 3.

---

### Phase 3: Vulnerability Discovery (Senior Auditor)
**Goal**: Find logic errors, state inconsistencies, and DeFi-specific vulnerabilities.

> ⚠️ **MANDATORY FILE OPERATIONS**:
> - At the START: **FIRST STEP** - **MUST** use `read_file` tool to read `.audit/blueprints/0_Setup.md`, `.audit/blueprints/1_Automated_Security_Scan.md`, and `.audit/blueprints/2_Understanding.md` - verify all exist and contain results. If files don't exist or are empty, STOP and re-run previous phases.
> - **🔴 CRITICAL REFERENCE FILES**: **MUST** read these reference files BEFORE starting vulnerability analysis:
>   - `references/DEFI_CHECKLIST.md` - DeFi protocol-specific vulnerability checklist (DEX/AMM, Lending, Vault, Router, Staking)
>   - `references/VULNERABILITY_PATTERNS.md` - Common vulnerability patterns and detection methods
> - **CRITICAL**: Reference Phase 2's function inventory and parameter semantics when analyzing vulnerabilities - explicitly cite Phase 2 findings in your analysis
> - During analysis: Re-read relevant code sections with Phase 3 focus (vulnerability patterns based on Phase 2 understanding)
> - At the END: Output complete Phase 3 content in conversation (with all vulnerability findings from all checklists), then **SAVE** to `.audit/blueprints/3_Vulnerability_Scan.md` using `write` tool
> - **VERIFICATION**: When starting Phase 4, **MUST** read Phase 3 file using `read_file` tool as the FIRST STEP - verify it exists and contains vulnerability findings
> - **DO NOT** proceed to Phase 4 until Phase 3 file is verified to exist and contains findings

> ⚠️ **CRITICAL**: You MUST reference your Phase 2 understanding (function inventory, parameter semantics, data flows) when performing vulnerability discovery. **MUST read Phase 2 results using `read_file` tool as the FIRST STEP**. Do NOT start vulnerability discovery without reading Phase 2 results.

#### Step 3.1: Storage & Global Context
1.  Identify Proxy/Upgradeable patterns. Check for **Storage Gaps** and **Collisions**.
2.  Map the global state variables (what tracks money?).
3.  Identify all external protocol integrations (routers, oracles, pools).

#### Step 3.2: Flow-Based Analysis
*Do NOT read functions in isolation. Analyze by Capability.*

**⚠️ CRITICAL: Use your Phase 2 understanding to guide this analysis.**

1.  **Group Functions** (based on your Phase 2 function inventory):
    *   *Deposit/Withdraw Flow*: `deposit`, `withdraw`, `mint`, `redeem`. (Check: Rounding, Inflation Attack, Share Manipulation)
    *   *Swap/Exchange Flow*: `swap`, `exchange`, path-based routing. (Check: **Token existence validation**, Path spoofing, Slippage bypass)
    *   *Liquidity Flow*: `addLiquidity`, `removeLiquidity`. (Check: LP token manipulation, Imbalanced adds)
    *   *Borrow/Repay Flow*: `borrow`, `repay`, `liquidate`. (Check: Interest accrual, Collateral validation)
    *   *Admin Flow*: `setParams`, `emergencyWithdraw`, `pause`. (Check: Timelocks, Access control, Rug vectors)
    *   *Reward Flow*: `claim`, `harvest`, `distribute`. (Check: Reward calculation, Double-claim)

2.  **⚠️ NEW: Internal Function Deep Dive**:
    Based on your Phase 2 function inventory, for EACH internal helper/routing function:
    
    a. **Parameter Validation Check**:
       - Review the parameter semantics you documented in Phase 2
       - Verify ALL assumptions are validated
       - Check if validations are done at the right place (entry point vs internal function)
       - Example: If function assumes a specific input format, is this validated BEFORE using it?
       - Example: If function assumes a token is registered, is this checked BEFORE accessing registry?
    
    b. **Cross-Function Consistency**:
       - Compare with similar functions you documented in Phase 2
       - Are validation rules consistent?
       - Example: If multiple functions perform similar operations, do they validate inputs the same way?
    
    c. **Edge Case Analysis**:
       - Based on your parameter semantics analysis, test edge cases
       - What if parameters are at boundaries (zero, max, empty arrays)?
       - What if parameters have unexpected values (address(0), unregistered tokens)?
       - Example: What if an array is empty? What if a required address is zero?

3.  **⚠️ NEW: Multi-Path Final Output Validation**:
    For entry point functions with multiple execution paths/branches (e.g., swap functions with different pool types, deposit functions with different token types):
    
    a. **Trace All Execution Paths**:
       - Identify ALL possible execution branches (different pool types, different token types, etc.)
       - For EACH branch, trace where the final output/result is stored
       - Check if each branch validates final output against user requirements (e.g., `amountOutMin`, minimum shares, etc.)
    
    b. **Unified Final Check**:
       - After ALL branches complete (before function returns), is there a UNIFIED check?
       - Example: After all execution paths complete, before emitting events, is there a final validation that the output meets user requirements?
       - ⚠️ **CRITICAL**: Even if individual internal functions check requirements internally, the entry point MUST verify the final output meets the user's minimum requirement
    
    c. **Branch Consistency**:
       - Compare final output validation across different branches
       - If one branch validates final output but others don't, this is an inconsistency vulnerability
       - Check if some branches rely on external contract checks while others don't - this creates inconsistent security guarantees
    
    d. **Fee-on-Transfer/Rebasing Consideration**:
       - If final output token is fee-on-transfer or rebasing, the actual received amount may differ from calculated amount
       - Does the final check account for this? Should it use `balanceAfter - balanceBefore` instead?

4.  **Action**: Document detailed findings in Phase 3 output, explicitly referencing your Phase 2 understanding (e.g., "Based on Phase 2 function inventory, function X...", "According to Phase 2 parameter semantics analysis...").

---

#### Step 3.3: DeFi-Specific Vulnerability Scan (CRITICAL)

> 🔴 **MANDATORY**: You **MUST** read and follow `references/DEFI_CHECKLIST.md` for this step.
> This checklist contains comprehensive vulnerability checks organized by protocol type.

**Execution Steps:**

1. **Read the Checklist**: Use `read_file` tool to read `references/DEFI_CHECKLIST.md`

2. **Identify Protocol Type**: Based on Phase 2 understanding, determine which sections apply:
   - 📋 Universal Checks (ALL protocols)
   - 🔄 DEX / AMM Specific (including V3+ Concentration Logic, V4 Hook Security)
   - 💰 Lending Protocol Specific
   - 🏦 Vault / Yield Specific
   - 🌉 Router / Aggregator Specific (including Slippage Protection, Flash Accounting, Call Injection)
   - 🔐 Staking Specific

3. **Systematic Check**: Go through EACH applicable checklist item:
   - Reference your Phase 2 function inventory
   - Cross-reference with Phase 2 parameter semantics
   - Document which items pass/fail with evidence

4. **Document Findings**: For each potential vulnerability found:
   - Reference the specific checklist item
   - Cite the affected function(s) from Phase 2 inventory
   - Provide code evidence
   - Assess severity

> ⚠️ **Common Gotchas**: Pay special attention to the "Common Gotchas Quick Reference" table at the end of the checklist - these are frequently missed vulnerabilities.

**Additional Cross-Cutting Checks (apply to ALL protocol types):**

| Check | Description |
|-------|-------------|
| **Access Control** | All admin functions protected, 2-step ownership, timelocks on critical changes |
| **Reentrancy** | CEI pattern, cross-function reentrancy, callback safety |
| **Input Validation** | All external inputs validated, array bounds, zero address checks |
| **Arithmetic Safety** | No precision loss, correct rounding direction, overflow protection |

---

#### Step 3.4: Solidity & Advanced Vulnerability Patterns (MANDATORY)

> 🔴 **MANDATORY**: You **MUST** read and follow `references/VULNERABILITY_PATTERNS.md` for this step.
> This reference contains 23 vulnerability patterns with code examples and detection methods.

**Execution Steps:**

1. **Read the Patterns**: Use `read_file` tool to read `references/VULNERABILITY_PATTERNS.md`

2. **Pattern Categories to Check**:
   - 🔴 **Critical**: Reentrancy, Mapping Default Values, Unprotected Init, Arbitrary Call
   - 🟠 **High**: Missing Access Control, Unchecked Returns, Oracle Manipulation
   - 🟡 **Medium**: Precision Loss, First Depositor Attack, Front-Running
   - 🔴 **Modern DeFi** (V4/EIP-1153/Aggregators):
     - Transient Storage Collision (EIP-1153)
     - Hook DoS (Uniswap V4)
     - Multicall msg.value Reuse
     - Permit2 Universal Approval Exploitation
     - Deadline Bypass / Hardcoded Deadline

3. **Use Quick Detection Checklist**: Apply the grep/search patterns from the reference file

4. **Document Findings**: For each pattern violation found, document with code evidence

> **⚠️ High-Priority Patterns for Modern DeFi**:
> - If contract uses `tstore`/`tload` → Check #19 Transient Storage Collision
> - If contract has hooks (beforeSwap, afterSwap) → Check #20 Hook DoS
> - If contract has `multicall` + `payable` → Check #21 msg.value Reuse
> - If contract integrates Permit2 → Check #22 Permit2 Exploitation
> - If contract calls external routers → Check #23 Deadline Bypass

---

#### Prompt Templates

**For DEX/Swap Analysis**:
> You are the **Senior DeFi Auditor** specializing in DEX protocols.
> **Task**: Analyze the swap/exchange functions: `[Function Names]`.
> 
> **⚠️ PREREQUISITE**: 
> - Review your Phase 2 function documentation for these functions
> - Read `references/VULNERABILITY_PATTERNS.md` for detection patterns
> - Read `references/DEFI_CHECKLIST.md` DEX/AMM section
> 
> **Focus**:
> 1. **Token Existence Validation**: For every token address from user input, verify it exists in the pool registry BEFORE using its pool index.
> 2. **Mapping Default Values (CRITICAL)**: `poolToken[pool][address]` returns 0 for unregistered addresses. Does code distinguish "not found" from "ID=0"?
> 3. **Path Validation**: Every token in the user-supplied path must be validated. Can attacker insert `address(0)` or arbitrary address?
> 4. **Slippage & Deadline**: Is `amountOutMin` enforced? Can it be 0? Is deadline user-provided or hardcoded `block.timestamp`?
> 5. **Internal Function Validation**: For each internal helper function, check if inputs are validated. Compare with your Phase 2 cross-function consistency analysis.
> 6. **Output Requirements**: Are user requirements actually enforced? ⚠️ **CRITICAL**: After ALL execution paths complete, is there a UNIFIED check that final output meets user requirements?
> 7. **Fee-on-Transfer/Rebasing**: Does code use `balanceAfter - balanceBefore` for actual received amounts?
> 8. **V3+/V4 Specific** (if applicable):
>    - Tick initialization DoS? Liquidity gross overflow?
>    - Hook return value validation? Callback origin verification?
>    - Transient storage (`tstore`) cleared in all paths?
>
> If a vulnerability is found, format it for the report with severity and PoC outline.

**⚠️ NEW: For Internal Function Analysis**:
> You are the **Senior DeFi Auditor** specializing in [Protocol Type].
> **Task**: Analyze ALL internal helper/routing functions from your Phase 2 function inventory: `[list internal functions from your Phase 2 documentation]`.
> 
> **CRITICAL Checklist for EACH internal function** (reference your Phase 2 documentation):
> 
> 1. **Parameter Semantics Validation (MANDATORY)**:
>    - Review the parameter semantics you documented in Phase 2
>    - For each parameter assumption, verify it's validated BEFORE use
>    - Example: If you documented "address(0) means native token", check: Is the native token handling validated BEFORE operations?
>    - Example: If you documented "token must be registered", check: Is registration verified BEFORE using token index?
> 
> 2. **Native Token Handling (if applicable)**:
>    - If function handles native tokens (from your Phase 2 analysis), trace the handling:
>    - Input is native: Does code require correct wrapping format?
>    - Output is native: Does code require correct unwrapping format?
>    - Are these checks done BEFORE wrapping/unwrapping operations?
>    - **Cross-reference**: Compare with your Phase 2 function documentation - are assumptions validated?
> 
> 3. **Input Length/Range Validation**:
>    - Based on your Phase 2 parameter analysis, what are the minimum/maximum required values?
>    - Does function check bounds before accessing array elements or using values?
>    - Example: Before accessing `array[1]`, is there a check that `array.length >= 2`?
>    - Example: Before using `amount`, is there a check that `amount > 0`?
> 
> 4. **Input Format Validation**:
>    - Does function check that inputs follow required format?
>    - Example: For swap paths, are adjacent tokens different?
>    - Example: For arrays, are elements in valid ranges?
> 
> 5. **Token/Address Existence**:
>    - For each token/address used, does function verify it exists in the registry/pool?
>    - Does it check BEFORE using indices or mappings (to prevent ID spoofing)?
> 
> 6. **Cross-Function Consistency**:
>    - Compare validation logic across similar functions (from your Phase 2 inventory)
>    - Use your Phase 2 cross-function consistency analysis
>    - Are validation rules consistent? Are edge cases handled the same way?
> 
> **Output Format**: For each finding, specify:
> - Function name and line numbers (from your Phase 2 inventory)
> - Missing validation type (reference your Phase 2 parameter semantics)
> - Potential impact (fund loss, operation failure, etc.)
> - Recommended fix with code example
> - Severity rating (Critical/High/Medium)

**For Router/Aggregator Analysis**:
> You are the **Senior DeFi Auditor** specializing in Router/Aggregator protocols.
> **Task**: Analyze the router/execute functions: `[Function Names]`.
> 
> **⚠️ PREREQUISITE**: 
> - Read `references/VULNERABILITY_PATTERNS.md` patterns #19-23 (Modern DeFi)
> - Read `references/DEFI_CHECKLIST.md` Router/Aggregator section
> 
> **Focus**:
> 1. **Multicall msg.value Reuse**: If `multicall` is `payable` with `delegatecall`, is `msg.value` tracked per call?
> 2. **Permit2 Exploitation**: If Router has Permit2 approval, can `execute` call arbitrary targets including Permit2?
> 3. **Deadline Handling**: Is deadline user-provided? Is `block.timestamp` used (defeats MEV protection)?
> 4. **Slippage Across Path**: Is `amountOutMin` checked at final output? Are intermediate hops validated?
> 5. **Transient Storage**: If using `tstore`, is state cleared in ALL exit paths (including early returns)?
> 6. **Sweep/Dust**: Are leftover tokens returned? Can next caller "pick up" residual funds?
> 7. **Arbitrary Call Injection**: Is `target` address whitelisted in dispatcher/execute logic?
>
> If a vulnerability is found, format it for the report with severity and PoC outline.

**For Admin Flow Analysis**:
> You are the **Senior DeFi Auditor** specializing in access control.
> **Task**: Analyze the admin functions: `[Function Names]`.
> **Focus**:
> 1. **Rug Vectors**: Can admin drain user funds? (emergency withdraw, fee manipulation, pause abuse)
> 2. **Timelocks**: Are critical parameter changes delayed?
> 3. **Two-Step Ownership**: Is ownership transfer a 2-step process?
> 4. **Input Validation**: Are all admin inputs validated (ranges, addresses)?
>
> If a vulnerability is found, format it for the report.

**MANDATORY OUTPUT**: Output complete Phase 3 content in conversation (formatted as markdown with all vulnerability findings). Then **IMMEDIATELY SAVE** this content to `.audit/blueprints/3_Vulnerability_Scan.md` using `write` tool. After saving, proceed to Phase 4.

---

### Phase 4: Defense & Verification (Lead Dev)
**Goal**: Filter False Positives with Logic/Math and assess real-world impact.

> ⚠️ **MANDATORY FILE OPERATIONS**:
> - At the START: **FIRST STEP** - **MUST** use `read_file` tool to read all previous blueprint files (`.audit/blueprints/0_Setup.md` through `.audit/blueprints/3_Vulnerability_Scan.md`) - verify all exist and contain results. If files don't exist or are empty, STOP and re-run previous phases.
> - During analysis: For EACH finding from Phase 3, re-read relevant code sections and perform verification
> - At the END: Output complete Phase 4 content in conversation (with verification results for each Phase 3 finding), then **SAVE** to `.audit/blueprints/4_Defense_Verification.md` using `write` tool
> - **VERIFICATION**: When generating final report, **MUST** read Phase 4 file using `read_file` tool as the FIRST STEP - verify it exists and contains verification results
> - **DO NOT** proceed to final report until Phase 4 file is verified to exist

> ⚠️ **PREREQUISITE**: All previous phase files must exist before starting Phase 4. **MUST read all previous phase files using `read_file` tool as the FIRST STEP**. Phase 4 verifies each finding from Phase 3.

**Operational Rules**:
1.  For each finding, define the **Safety Invariant** it violates.
2.  Assess real-world impact: "What's the maximum extractable value?"
3.  Consider: Can this be combined with flash loans for bigger impact?
4.  Check if exploit relies on impossible conditions (e.g., minting infinite USDT).

**Prompt per finding**:
> You are the **Defense Attorney**.
> **Input**: Finding ID `[ID]`.
> **Task**:
> 1. **Define Invariant**: e.g., `sum(balances) == totalSupply`, `totalBorrowed <= totalDeposited`, `only registered tokens can be swapped`.
> 2. **Assess Impact**: What is the maximum loss? Dust or significant?
> 3. **Feasibility Check**: Is the attack economically viable after gas/flash loan fees?
> 4. **Confirm/Dispute**: If disputing, provide mathematical proof or code limitation explanation.
>
> **Output**: Final Verdict with severity rating.

#### Severity Classification

| Severity | Impact | Likelihood | Examples |
|----------|--------|------------|----------|
| **Critical** | Direct fund loss, protocol insolvency | High (easy to exploit) | Reentrancy drains, unprotected init, token ID spoofing, Permit2 arbitrary call, multicall msg.value reuse, transient storage leak |
| **High** | Significant damage, partial fund loss | Medium-High | Access control bypass, oracle manipulation, unchecked transfers, Hook DoS, deadline bypass, slippage bypass |
| **Medium** | Conditional impact, requires setup | Medium | Precision loss, front-running, timing issues, callback origin not verified, sweep/dust residue |
| **Low** | Minor issues, unlikely exploitation | Low | Missing events, naming conventions, gas optimizations, hardcoded slippage |
| **Informational** | Best practices, no security impact | N/A | Code quality, documentation gaps |

**MANDATORY OUTPUT**: Output complete Phase 4 content in conversation (formatted as markdown with all verification results). Then **IMMEDIATELY SAVE** this content to `.audit/blueprints/4_Defense_Verification.md` using `write` tool. After saving, proceed to generate the final merged report.

---

## 🧠 Core Principles
1.  **Sequential Execution**: ⚠️ **CRITICAL** Execute phases one at a time. Complete Phase N fully (including outputting content and **SAVING to file using `write` tool**) before starting Phase N+1. Do NOT combine multiple phases in a single execution.
2.  **File-Based Workflow**: ⚠️ **CRITICAL** Each phase must be **SAVED by AI using `write` tool** to a separate file. **ALWAYS read previous phase files using `read_file` tool as the FIRST STEP** when starting a new phase. **ALWAYS verify files exist and contain content** before proceeding. This ensures continuity and forces careful review of previous findings.
3.  **Code Re-Reading**: ⚠️ **CRITICAL** Re-read relevant code files for each phase. Each phase has different focus (architecture, security patterns, deep understanding, vulnerability discovery, verification), so code must be re-read with that focus in mind.
4.  **Understand Before Auditing**: ⚠️ **NEW** Complete Phase 2 (Contract Understanding) before vulnerability discovery. You cannot effectively audit what you don't understand.
5.  **Context Groups**: Audit capabilities, not lines of code. Use your Phase 2 function inventory to group related functions.
6.  **Parameter Semantics First**: ⚠️ **NEW** Understand what each parameter means before checking validation. Use your Phase 2 parameter semantics analysis.
7.  **DeFi-Specific Thinking**: Always ask "What if this is part of a flash loan? What if the price is manipulated?"
8.  **Stateful & Append-Only**: Never delete Phase 3 findings. Only refine or dispute them in Phase 4.
9.  **Invariant-Based Verification**: Every critical function should have a clear invariant it must maintain. Use your Phase 2 understanding to identify invariants.
10. **Internal Function Parity**: ⚠️ **NEW** Internal functions deserve the same scrutiny as public functions. Use your Phase 2 function inventory to ensure all functions are audited.
11. **Error Handling**: ⚠️ **NEW** If a required blueprint file doesn't exist or is empty when starting a new phase, STOP immediately and re-run the previous phase. Do NOT proceed without verification.
12. **Tool Availability**: ⚠️ **NEW** If automated tools (Slither, Mythril) are unavailable, perform manual pattern-based analysis instead. Document findings with `[MANUAL]` tag instead of `[AUTO]`.

---

## 📤 Final Deliverable (Mandatory)

After completing Phase 4, you **MUST** finalize and output the Audit Report.

### Phase Final: Report Generation

> ⚠️ **MANDATORY FILE OPERATIONS**:
> - **MUST** use `read_file` tool to read `.audit/blueprints/3_Vulnerability_Scan.md` and `.audit/blueprints/4_Defense_Verification.md` (primary sources)
> - **OPTIONAL**: Reference `.audit/blueprints/0_Setup.md`, `.audit/blueprints/1_Automated_Security_Scan.md`, and `.audit/blueprints/2_Understanding.md` for context
> - Verify all required files exist before generating final report

1.  **MANDATORY**: Use `read_file` tool to read `.audit/blueprints/3_Vulnerability_Scan.md` and `.audit/blueprints/4_Defense_Verification.md` (primary sources)
2.  **OPTIONAL**: Reference `.audit/blueprints/0_Setup.md`, `.audit/blueprints/1_Automated_Security_Scan.md`, and `.audit/blueprints/2_Understanding.md` for context (protocol type, architecture, understanding summary)
3.  **FOCUS**: The final report should center around Phase 3 (Vulnerability Scan) and Phase 4 (Defense & Verification) comprehensive results
4.  **Structure** (follow this exact structure):
    ```markdown
    # Smart Contract Security Audit Report
    
    **Date:** [Date]
    **Target Protocol / Contracts:** [Protocol Name]
    **Protocol Type:** [From Phase 0]
    **Auditor:** AI Security Auditor (DeFi Smart Contract Audit Skill v2.8.0)
    
    ## Audit Phase Status
    - [x] Phase 0: Architecture & Protocol Classification
    - [x] Phase 1: Automated Security Scan
    - [x] Phase 2: Contract Understanding
    - [x] Phase 3: Vulnerability Discovery
    - [x] Phase 4: Defense & Verification
    
    ## 📊 Executive Summary
    **Overall Risk Level:** [🔴 Critical | 🟠 High | 🟡 Medium | 🟢 Low]
    
    | Severity | Count |
    |----------|-------|
    | Critical | [N] |
    | High | [N] |
    | Medium | [N] |
    | Low | [N] |
    | Informational | [N] |
    
    **Key Findings Summary:**
    [Brief summary of critical and high findings from Phase 3 & 4]
    
    ## 🏛️ Protocol Overview
    [Brief summary from Phase 0: protocol type, key contracts, architecture]
    
    ## 🔍 Detailed Findings
    [All findings from Phase 3, with Phase 4 verification results integrated]
    
    ### [C-01] Finding Title
    **Severity:** 🔴 Critical
    **Status:** [Confirmed | Disputed | Mitigated]
    **Location:** [Contract:Line]
    
    [Full finding description from Phase 3]
    
    #### Phase 4: Defense & Verification
    [Verification results from Phase 4]
    
    [Repeat for all findings]
    
    ## 📝 Recommendations
    [Prioritized recommendations based on Phase 4 impact assessment]
    
    ## 📚 Appendix
    ### A. Files Reviewed
    [List from Phase 0]
    
    ### B. Methodology
    [Reference to all phases]
    
    ### C. Vulnerability Classification
    [Severity definitions]
    ```
5.  **Finalize**: Review and clean up the report to ensure:
    *   All findings from Phase 3 are included with their Phase 4 verification status
    *   The "Findings Summary" table is accurate and up-to-date
    *   Each finding has severity, impact, and recommendation
    *   **Severity is justified** with impact and likelihood from Phase 4
    *   All cross-references are accurate
    *   Executive summary accurately reflects Phase 3 & 4 results
6.  **Output**: Output complete final report in conversation (formatted as markdown following the structure above)
7.  **SAVE**: **IMMEDIATELY SAVE** this content to `.audit/Audit-Report.md` using `write` tool. This is the final comprehensive audit report focusing on vulnerabilities (Phase 3) and their verification (Phase 4).

---

## 📚 Reference: Common DeFi Attack Patterns

For quick reference during audit:

| Attack | Typical Target | Key Indicators |
|--------|---------------|----------------|
| Flash Loan Attack | Price oracles, yield farming | Spot price used, single-block manipulation possible |
| Sandwich Attack | DEX swaps | No slippage protection, predictable transactions |
| Reentrancy | Any external call before state update | `call()`, `transfer()`, callbacks before state change |
| **Read-Only Reentrancy** | Curve/Balancer integrations | View function reads state during callback |
| **ERC Token Callback** | ERC777/721/1155 receivers | `tokensReceived`, `onERC721Received` without reentrancy guard |
| First Depositor | Vaults, share-based pools | `shares = deposit * totalShares / totalAssets` with 0 initial |
| **Donation Attack** | ERC4626 Vaults | Direct token transfer inflates share price |
| Oracle Manipulation | Lending, leveraged positions | `getReserves()`, TWAP window too short |
| Token Approval Front-run | Any `approve()` | Changing approval from non-zero to non-zero |
| Governance Attack | DAOs, parameter changes | Low quorum, flash loan votes |
| Token ID Spoofing | DEX with token registries | Mapping returns 0 for unregistered, colliding with token ID 0 |
| **Unsafe Downcast** | Any type conversion | `uint256` -> `uint128` without bounds check |
| **Force-feeding ETH** | Contracts using `balance` | `selfdestruct` can force ETH, breaking `address(this).balance` checks |
| **Signature Replay** | Permit, meta-tx, multi-chain | Missing `chainId`, `nonce`, or contract address in signed data |
