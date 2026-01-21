---
name: Sunhat TRON Development
description: The official detailed guide for developing, testing, and deploying TRON smart contracts using the Sunhat toolkit.
---

# Sunhat TRON Development Skill

This skill enables you to develop, test, and deploy smart contracts on the TRON network.

**Rule:** Do not memorize the details of every task. Only read the specific workflow file relevant to your current objective.

## Capabilities

| Objective | Workflow File | Description |
| :--- | :--- | :--- |
| **Initialize Project** | [sunhat-init.md](workflows/sunhat-init.md) | Setup new project structure, config, and env. |
| **Compile Contracts** | [sunhat-compile.md](workflows/sunhat-compile.md) | Compile Solidity/Vyper with TRON settings. |
| **Run Tests** | [sunhat-test.md](workflows/sunhat-test.md) | Run Foundry (Solidity) or Hardhat (JS) tests. |
| **Deploy to Network** | [sunhat-deploy.md](workflows/sunhat-deploy.md) | Deploy contracts to Mainnet/Nile/Shasta. |

## Quick Reference

- **CLI Tool**: `sunhat` (implicitly wraps Hardhat)
- **Config**: `hardhat.config.ts`
- **Networks**: `tron` (alias for configured TRON network)
