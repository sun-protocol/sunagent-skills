---
name: DeFi Smart Contract Audit
description: A rigorous, adversarial DeFi smart contract audit process with specialized checks for DEX, AMM, Lending, and token interactions.
version: "2.0.0"
updated: "2026-01"
---

# DeFi Smart Contract Audit Skill (Advanced)

This skill executes a high-precision security audit using a "Team of Rivals" approach, **specialized for DeFi protocols**.

## 📂 Skill Resources

| Resource | Path | Description |
|----------|------|-------------|
| **Audit Report Template** | `resources/audit_report_template.md` | Template for Phase 0 initialization |
| **Vulnerability Patterns** | `references/VULNERABILITY_PATTERNS.md` | Solidity vulnerability code examples |
| **DeFi Checklist** | `references/DEFI_CHECKLIST.md` | Protocol-specific audit checklists |

## 🚀 Workflow Overview

1.  **Phase 0 (Architect)**: Map architecture, identify DeFi protocol type, and verify documentation.
2.  **Phase 1 (Automated Scan)**: Run static analysis tools (Slither, Mythril) to catch low-hanging fruit.
3.  **Phase 2 (Senior Auditor)**:
    *   Step 2.1: Storage Layout & Global Scan.
    *   Step 2.2: **Flow-Based** Function Analysis.
    *   Step 2.3: **DeFi-Specific Vulnerability Scan** (CRITICAL).
    *   Step 2.4: **Solidity Gotchas Checklist**.
4.  **Phase 3 (Defense)**: Verify findings using **Invariants** and assess real-world impact.

---

## 🛠️ Instructions

### Phase 0: Architecture & Protocol Classification
1.  **Initialize**: Copy content from `resources/audit_report_template.md` -> `Audit-Report.md` in the target project root.
2.  **Identify Protocol Type**: Classify the DeFi protocol:
    *   `DEX/AMM` - Token swaps, liquidity pools (Uniswap, Curve, etc.)
    *   `Lending` - Borrow/Lend protocols (Aave, Compound, etc.)
    *   `Yield/Vault` - Yield aggregators, vaults (Yearn, etc.)
    *   `Stablecoin/PSM` - Stablecoin mechanisms, PSM modules
    *   `Bridge/Router` - Cross-chain or multi-protocol routers
    *   `Staking` - Token staking, reward distribution
3.  **Doc Check**: Read `README.md` or specs (if exists). Compare against the code outline.
    *   **If README is empty or generic**: Mark "Documentation vs Code Discrepancies" as `N/A - No meaningful spec provided` and proceed.
4.  **Action**: Fill "Phase 0" in report, specifically noting protocol type and any **Documentation Mismatches**.

---

### Phase 1: Automated Security Scan
**Goal**: Use static analysis tools to find common vulnerabilities before manual review.

> **Reference**: This phase uses methodology from the `smart-contract-security` skill.

#### Step 1: Run Static Analysis Tools

**Slither** (Fast vulnerability detection):
```bash
# Basic scan
slither . --exclude-dependencies

# JSON output for parsing
slither . --json slither-report.json --exclude-dependencies

# Focus on high-impact detectors only
slither . --detect reentrancy-eth,reentrancy-no-eth,arbitrary-send,controlled-delegatecall,suicidal,unprotected-upgrade
```

**Mythril** (Symbolic execution - deeper analysis):
```bash
# Single contract analysis
myth analyze contracts/Contract.sol --solc-json mythril.config.json

# Full project
myth analyze . --solc-json mythril.config.json
```

**Semgrep** (Custom rules for smart contracts):
```bash
semgrep --config "p/smart-contracts" .
```

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

**Action**: Add significant automated findings to `Audit-Report.md` with `[AUTO]` tag.

---

### Phase 2: Vulnerability Discovery (Senior Auditor)
**Goal**: Find logic errors, state inconsistencies, and DeFi-specific vulnerabilities.

#### Step 2.1: Storage & Global Context
1.  Identify Proxy/Upgradeable patterns. Check for **Storage Gaps** and **Collisions**.
2.  Map the global state variables (what tracks money?).
3.  Identify all external protocol integrations (routers, oracles, pools).

#### Step 2.2: Flow-Based Analysis
*Do NOT read functions in isolation. Analyze by Capability.*
1.  **Group Functions**:
    *   *Deposit/Withdraw Flow*: `deposit`, `withdraw`, `mint`, `redeem`. (Check: Rounding, Inflation Attack, Share Manipulation)
    *   *Swap/Exchange Flow*: `swap`, `exchange`, path-based routing. (Check: **Token existence validation**, Path spoofing, Slippage bypass)
    *   *Liquidity Flow*: `addLiquidity`, `removeLiquidity`. (Check: LP token manipulation, Imbalanced adds)
    *   *Borrow/Repay Flow*: `borrow`, `repay`, `liquidate`. (Check: Interest accrual, Collateral validation)
    *   *Admin Flow*: `setParams`, `emergencyWithdraw`, `pause`. (Check: Timelocks, Access control, Rug vectors)
    *   *Reward Flow*: `claim`, `harvest`, `distribute`. (Check: Reward calculation, Double-claim)
2.  **Action**: detailed finding in `Audit-Report.md`.

---

#### Step 2.3: DeFi-Specific Vulnerability Checklist (CRITICAL)

**You MUST check ALL items in this checklist based on protocol type.**

##### 🔄 DEX/AMM Vulnerabilities

| Vulnerability | Description | How to Check |
|--------------|-------------|--------------|
| **Token ID/Index Spoofing** | Mapping returns default 0 for unregistered tokens | Verify token exists in pool BEFORE using its index. `poolToken[pool][fake_addr] == 0` can collide with real token 0 |
| **Path Validation Missing** | User-supplied swap path not validated | Each token in path must exist in respective pool. Check: Can attacker insert arbitrary address? |
| **Slippage Protection Bypass** | `amountOutMin` can be set to 0 or manipulated | Is slippage enforced? Can it be frontrun? |
| **Price Oracle Manipulation** | Spot price used instead of TWAP | If using `getReserves()` or single-block price, vulnerable to flash loan |
| **Fee-on-Transfer Token** | Tokens that take fee on transfer break accounting | Check: `balanceAfter - balanceBefore` vs `amount`. Does code handle deflationary tokens? |
| **Rebasing Token** | Tokens that change balance automatically | Check: Balance can change without transfer. Does code snapshot balances? |
| **Return Value Ignored** | `transfer()` returning false not checked | SafeERC20 should be used, or require return value |

##### 💰 Lending Protocol Vulnerabilities

| Vulnerability | Description | How to Check |
|--------------|-------------|--------------|
| **Interest Accrual Timing** | Interest not updated before state changes | `accrueInterest()` must be called BEFORE any borrow/repay |
| **Collateral Factor Manipulation** | Changing collateral factor affects existing positions | Can admin rug by setting factor to 0? |
| **Liquidation Threshold** | Self-liquidation or liquidation griefing | Can user block liquidation? Can liquidator drain excess? |
| **Oracle Stale Price** | Using outdated oracle price | Check timestamp/round validation on Chainlink oracles |
| **Borrow Limit Bypass** | Borrowing more than collateral allows | Re-enter before borrow limit check? Flash loan collateral? |

##### 🏦 Vault/Yield Vulnerabilities

| Vulnerability | Description | How to Check |
|--------------|-------------|--------------|
| **First Depositor Inflation Attack** | First depositor can manipulate share price | Donate tokens before first deposit to inflate share price |
| **Share Rounding Down** | Attacker gets 0 shares for deposit | `shares = deposit * totalShares / totalAssets` rounds down |
| **Harvest Sandwich** | MEV bots sandwich harvest transactions | Are rewards distributed atomically? |
| **Strategy Migration Risk** | Funds stuck during migration | Emergency withdrawal path available? |

##### 🌉 Router/Bridge Vulnerabilities

| Vulnerability | Description | How to Check |
|--------------|-------------|--------------|
| **Pool/Token Registry Bypass** | Using fake pool or token address | All external addresses must be validated against registry |
| **Cross-Pool Arbitrage** | Inconsistent pricing across integrated pools | Can user route through pools with stale prices? |
| **Deadline Bypass** | Transaction deadline not enforced | Is `deadline` actually checked? Can be set to `type(uint256).max`? |
| **Recipient Manipulation** | User can set arbitrary recipient | Check: Can user send funds to different address than expected? |

##### 🔐 Access Control & Admin

| Vulnerability | Description | How to Check |
|--------------|-------------|--------------|
| **Missing Access Control** | Critical function lacks `onlyOwner`/`onlyAdmin` | All parameter changes and withdrawals must be protected |
| **Single-Step Ownership Transfer** | Typo in `transferOwnership` loses contract | Should use 2-step: propose -> accept |
| **No Timelock on Critical Changes** | Parameters changed instantly | Fee changes, pool additions should have delay |
| **Admin Can Rug** | Owner can drain all funds | Check `emergencyWithdraw`, fee manipulation, pause abuse |

---

#### Step 2.4: Solidity Gotchas Checklist (MANDATORY)
*These are common Solidity-specific vulnerability patterns that MUST be checked:*

| Pattern | Description | How to Check |
|---------|-------------|--------------|
| **Mapping Default Values** | `mapping` returns `0`/`false`/`address(0)` for non-existent keys | Look for `mapping[key]` reads without prior existence check. Ask: "What if this key was never set?" |
| **Token/Address Existence** | Using mapping lookups that return 0 for unregistered tokens | Verify that all token addresses passed as input are validated against a whitelist or registry |
| **ID Collision via Default** | When `ID=0` is valid, unregistered items return same ID as first item | Check if ID `0` is used as a valid identifier. If so, require explicit existence check |
| **address(0) Handling** | Native ETH/TRX often represented as `address(0)` | Ensure `address(0)` is handled explicitly and cannot be confused with "not found" |
| **Unchecked Return Values** | External calls that return success/failure | Verify all external calls check return values |
| **Reentrancy** | State modified after external call | Check: CEI pattern followed? ReentrancyGuard used? Cross-function reentrancy? |
| **Integer Overflow (pre-0.8)** | Arithmetic without SafeMath | If Solidity < 0.8, require SafeMath for all arithmetic |
| **Precision Loss** | Dividing before multiplying | `a / b * c` loses precision. Should be `a * c / b` |
| **Block.timestamp Manipulation** | Miners can manipulate timestamp slightly | Is there a 15-second window issue? |

> **Example Vulnerability (Token ID Spoofing)**:
> ```solidity
> uint128 tokenId = poolToken[pool][userInputAddress]; // Returns 0 if not registered!
> ```
> If token at index 0 exists, attacker can spoof it by passing ANY unregistered address.

---

#### Prompt Templates

**For DEX/Swap Analysis**:
> You are the **Senior DeFi Auditor** specializing in DEX protocols.
> **Task**: Analyze the swap/exchange functions: `[Function Names]`.
> **Focus**:
> 1. **Token Existence Validation**: For every token address from user input, verify it exists in the pool registry BEFORE using its pool index.
> 2. **Mapping Default Values (CRITICAL)**: `poolToken[pool][address]` returns 0 for unregistered addresses. Does code distinguish "not found" from "ID=0"?
> 3. **Path Validation**: Every token in the user-supplied path must be validated. Can attacker insert `address(0)` or arbitrary address?
> 4. **Slippage**: Is `amountOutMin` actually enforced? Can it be bypassed?
> 5. **Fee-on-Transfer**: Does code use `balanceAfter - balanceBefore` for actual received amounts?
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

---

### Phase 3: Defense & Verification (Lead Dev)
**Goal**: Filter False Positives with Logic/Math and assess real-world impact.

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
| **Critical** | Direct fund loss, protocol insolvency | High (easy to exploit) | Reentrancy drains, unprotected init, arbitrary token ID spoofing |
| **High** | Significant damage, partial fund loss | Medium-High | Access control bypass, oracle manipulation, unchecked transfers |
| **Medium** | Conditional impact, requires setup | Medium | Precision loss, front-running, timing issues |
| **Low** | Minor issues, unlikely exploitation | Low | Missing events, naming conventions, gas optimizations |
| **Informational** | Best practices, no security impact | N/A | Code quality, documentation gaps |

---

## 🧠 Core Principles
1.  **Context Groups**: Audit capabilities, not lines of code.
2.  **DeFi-Specific Thinking**: Always ask "What if this is part of a flash loan? What if the price is manipulated?"
3.  **Stateful & Append-Only**: Never delete Phase 2 findings. Only refine or dispute them.
4.  **Invariant-Based Verification**: Every critical function should have a clear invariant it must maintain.

---

## 📤 Final Deliverable (Mandatory)

After completing Phase 3, you **MUST** finalize and output the Audit Report.

### Phase Final: Report Generation
1.  **Finalize**: Review and clean up the `Audit-Report.md` file to ensure:
    *   All checkboxes in the "Audit Phase Status" section are marked complete.
    *   The "Findings Summary" table is accurate and up-to-date.
    *   Each finding has severity, impact, and recommendation.
    *   **Severity is justified** with impact and likelihood.
2.  **Output Location**: Save the final report to the **target project's root directory**.
    *   File Path: `<Project-Root>/Audit-Report.md`
3.  **Notify User**: Inform the user that the audit is complete and provide the path to the final report.

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

