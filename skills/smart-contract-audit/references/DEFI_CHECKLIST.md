# DeFi Protocol Audit Checklist

> Comprehensive checklist organized by protocol type.
> Check all applicable items during Phase 1 manual review.

---

## 📋 Universal Checks (All Protocols)

### Access Control
- [ ] All admin functions have proper access modifiers (`onlyOwner`, `onlyAdmin`)
- [ ] Ownership transfer is 2-step (propose → accept)
- [ ] Critical parameter changes have timelock
- [ ] No unprotected `initialize()` function
- [ ] `selfdestruct` / `delegatecall` properly protected

### Reentrancy
- [ ] CEI pattern followed (Checks-Effects-Interactions)
- [ ] `nonReentrant` modifier on state-changing external functions
- [ ] Cross-function reentrancy considered (multiple functions share state)

### Token Handling
- [ ] Use `SafeERC20` for all token operations
- [ ] Handle fee-on-transfer tokens (use `balanceAfter - balanceBefore`)
- [ ] Handle rebasing tokens if applicable
- [ ] No assumption about token decimals (query dynamically)
- [ ] `address(0)` handled explicitly for native currency

### Arithmetic
- [ ] No precision loss (multiply before divide)
- [ ] Rounding direction favors protocol (rounds down for withdrawals)
- [ ] Safe math for Solidity < 0.8.0
- [ ] No overflow in unchecked blocks

### Input Validation
- [ ] All external inputs validated
- [ ] Array length checks to prevent DoS
- [ ] Zero address checks where applicable
- [ ] Reasonable bounds on numeric inputs

---

## 🔄 DEX / AMM Specific

### Token Registry
- [ ] **Token existence validated before swap** (mapping default value check!)
- [ ] Token IDs/indices verified against registry
- [ ] Pool address validated from factory/registry
- [ ] Path tokens validated at each hop

### Swap Mechanics
- [ ] Slippage protection enforced (`amountOutMin`)
- [ ] Deadline parameter actually checked
- [ ] Recipient address validated
- [ ] Fee calculation correct and no overflow

### Price / Oracle
- [ ] TWAP used instead of spot price (flash loan resistant)
- [ ] Oracle staleness check (timestamp validation)
- [ ] Price manipulation resistance during swaps
- [ ] Reserve ratios cannot be manipulated in single tx

### Liquidity
- [ ] LP token minting/burning calculations correct
- [ ] No inflation attack on first deposit
- [ ] Imbalanced liquidity adds handled properly
- [ ] Remove liquidity doesn't leave dust

---

## 💰 Lending Protocol Specific

### Collateral
- [ ] Collateral factor bounds checked (not > 100%)
- [ ] Collateral valuation uses secure oracle
- [ ] Collateral cannot be withdrawn while borrowed
- [ ] Cross-collateral calculated correctly

### Borrowing
- [ ] Borrow limit enforced before and after borrow
- [ ] Interest accrues before any state change
- [ ] Borrow cannot exceed available liquidity
- [ ] Health factor calculated correctly

### Liquidation
- [ ] Liquidation threshold < collateral factor
- [ ] Self-liquidation prevented or handled
- [ ] Liquidation bonus reasonable (no drain)
- [ ] Partial liquidation calculated correctly
- [ ] Underwater positions can always be liquidated

### Interest
- [ ] Interest rate model bounded
- [ ] Accrual frequency reasonable
- [ ] Utilization rate calculated correctly
- [ ] No overflow in long-term interest calculation

---

## 🏦 Vault / Yield Specific

### Share Calculation
- [ ] **First depositor attack mitigated** (virtual shares or minimum)
- [ ] Share price cannot be manipulated by donation
- [ ] Rounding favors vault (down on deposit, up on withdraw)
- [ ] Total assets includes pending rewards

### Strategy
- [ ] Strategy changes have timelock
- [ ] Emergency withdrawal path available
- [ ] Harvest does not favor specific users
- [ ] Slippage on strategy actions

### Rewards
- [ ] Reward calculation overflow-safe
- [ ] No double-claiming rewards
- [ ] Reward distribution gas-bounded
- [ ] Staking snapshot timing correct

---

## 🌉 Router / Aggregator Specific

### Path Validation
- [ ] **All tokens in path validated against pools**
- [ ] Each hop uses correct pool
- [ ] No arbitrary pool/token injection
- [ ] Path length bounded

### Multi-Protocol
- [ ] Each external call validated
- [ ] Return values from external protocols checked
- [ ] Slippage across entire path
- [ ] Deadline enforced through entire route

### Funds Handling
- [ ] Leftover tokens returned to user
- [ ] Native currency (ETH/TRX) wrapped/unwrapped correctly
- [ ] No stuck funds in router
- [ ] Recipient address from caller, not parameter

---

## 🔐 Staking Specific

### Reward Distribution
- [ ] Reward per token calculation precision
- [ ] Rewards accrue before any stake/unstake
- [ ] No reward dilution through flash stake
- [ ] Reward update on all entry/exit points

### Lock Periods
- [ ] Lock period enforced on withdraw
- [ ] Early withdrawal penalty calculated correctly
- [ ] Lock cannot be extended maliciously
- [ ] Penalty distribution fair

### Governance
- [ ] Voting power snapshot at correct time
- [ ] No flash loan voting
- [ ] Delegation tracked correctly
- [ ] Quorum reasonable

---

## ⚠️ Common Gotchas Quick Reference

| Issue | How to Miss It | How to Find It |
|-------|---------------|----------------|
| Mapping default 0 | Assume key exists | "What if key never set?" |
| address(0) dual use | Native currency vs not-found | Check all address(0) paths |
| ID = 0 valid | First item has ID 0 | Check if 0 is valid state |
| Stale oracle | Works in tests | Check timestamp validation |
| First depositor | Works with 1+ depositors | Test with empty vault |
| Reentrancy | No obvious callback | Check ERC777, receive(), external calls |

---

## ✅ Sign-Off Checklist

Before marking audit complete:

- [ ] All items above checked for applicable protocol type
- [ ] Automated tools run and findings triaged
- [ ] Each finding has severity and recommendation
- [ ] Report reviewed for accuracy
- [ ] Recommendations prioritized

---

*Checklist for DeFi Smart Contract Audit Skill*
