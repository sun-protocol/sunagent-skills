# DeFi 智能合约审计 Skill - 核心指南

**版本**: v2.10.0  
**更新日期**: 2026-06

这个版本针对 AI Token 消耗做了优化：保留多阶段审计方法，但中间阶段只作为内部工作笔记，不再输出或保存 `.audit/blueprints/` 文件。最终只生成一个审计报告文件，并默认省略低价值过程细节。

## 核心理念

### 1. 理解优先

在发现漏洞之前，必须先理解合约架构、函数目的、数据流、资金流和参数语义。Phase 2 仍然是强制阶段，但其结果只保留为内部笔记，不写成中间文件。

### 2. 双分析方式

Phase 3 使用两种互补方式：

- **Function-Based**：逐个函数检查参数验证、访问控制、重入、算术、外部调用等问题。
- **Flow-Based**：按业务流程检查跨函数状态一致性、资金流、滑点保护、回调、router/multicall 等系统性风险。

### 3. 最小输出策略

只允许写入最终报告：

```text
.audit/Audit-Report.md
```

不再生成：

```text
.audit/blueprints/0_Setup.md
.audit/blueprints/1_Automated_Security_Scan.md
.audit/blueprints/2_Understanding.md
.audit/blueprints/3_Vulnerability_Scan.md
.audit/blueprints/4_Defense_Verification.md
```

这样可以明显减少 output token，也避免后续阶段反复读取长篇中间报告造成 token 叠加。

### 4. 省略优先策略

默认省略以下内容，除非用户明确要求：

- 完整 function inventory。
- 完整 checklist pass/fail 表。
- 完整 vulnerability pattern pass/fail 表。
- Phase 统计、验证分类统计、重读效率统计。
- Slither / Semgrep / Mythril / Forge / Hardhat 原始输出。
- 工具 JSON 原文。
- 误报的长篇解释。
- 大量 Low / Informational finding 展开。

默认保留：

- 高风险合约、函数、业务流程。
- 已确认漏洞的代码引用。
- 简洁 coverage summary。
- 工具是否运行及高价值确认结果。
- 剩余 scope 限制。

### 5. 参考文件驱动

漏洞发现阶段仍会使用：

- `references/DEFI_CHECKLIST.md`：协议类型相关检查清单。
- `references/VULNERABILITY_PATTERNS.md`：Solidity / DeFi 漏洞模式。
- `resources/audit_report_template.md`：最终报告模板。

## 审计流程

```text
Phase 0 架构分类
  -> Phase 1 自动化扫描（可选）
  -> Phase 2 合约理解（强制）
  -> Phase 3 漏洞发现
  -> Phase 4 验证与影响评估
  -> Final Report
```

### Phase 0: 架构与协议分类

目标：识别协议类型、关键合约、角色权限、外部依赖、资金流和文档差异。

内部记录：

- 协议类型和链。
- 关键合约及职责。
- 资产托管点和资金流。
- privileged roles / owner / admin / keeper。
- 外部协议、oracle、token、router、pool 等依赖。
- 文档缺失或文档与代码不一致的地方。

### Phase 1: 自动化安全扫描（可选）

目标：使用工具辅助发现低成本问题。工具不可用、依赖安装成本高、或用户要求轻量审计时可跳过。

建议：

- 默认优先考虑 Slither、Semgrep、编译和测试。
- Mythril 只用于特定合约或特定疑点，不默认全项目跑。

内部记录：

- 哪些工具有跑，哪些跳过以及原因。
- 编译/测试结果。
- 高价值工具发现。
- 需要人工确认的自动化发现。

### Phase 2: 合约理解（强制）

目标：建立足够的业务和代码理解，避免只靠模式匹配。

重点：

- 高风险函数的目的、权限、参数语义、状态变化、资产变化、外部调用。
- 资金流：入口、内部记账、出口、卡资金风险。
- 状态机：关键状态变量、状态转换、不变量。
- 依赖图：函数之间如何调用，哪些路径最关键。
- 相似函数的一致性：例如 deposit/withdraw、swap exact in/out、native/token 分支。

高风险函数包括：资产转移、记账、权限、升级、oracle、签名、callback、外部调用、approval、swap、mint/burn、清算、奖励分发相关函数。低风险 helper 只记录异常假设或问题。这部分不再输出完整函数清单，除非用户明确要求。

### Phase 3: 漏洞发现

目标：发现逻辑错误、状态不一致、DeFi 特定漏洞。

使用：

- Function-Based Analysis。
- Flow-Based Analysis。
- `DEFI_CHECKLIST.md`。
- `VULNERABILITY_PATTERNS.md`。

内部记录每个候选问题：

- Finding ID 和标题。
- 严重性候选。
- 位置和代码引用。
- 漏洞描述。
- 攻击路径。
- 影响。
- 修复建议。
- 验证状态：证据完整、需要定向复核、不确定。

Checklist 和漏洞模式只记录类别级覆盖：

- 哪些类别已检查。
- 哪些类别发现确认问题。
- 哪些类别因不在 scope 内跳过。

不输出逐项 pass/fail 表，除非用户明确要求。

### Phase 4: 验证与影响评估

目标：过滤误报，给出可辩护的严重性。

对每个候选问题检查：

- 违反了什么安全不变量。
- 攻击是否真实可行。
- 最大损失或影响范围。
- 是否可被 flash loan、oracle manipulation、MEV、reentrancy 放大。
- 是否存在缓解条件或外部假设。

只有确认的问题进入最终报告主体。

Low / Informational 问题默认压缩成短表格；不为每个小问题展开完整 finding。

## 最终输出

最终只写：

```text
.audit/Audit-Report.md
```

报告应包含：

- Executive Summary。
- Scope and Methodology。
- Severity Summary。
- Confirmed Findings，按 Critical / High / Medium / Low / Informational 分组。
- 每个 finding 包含：标题、严重性、影响文件/函数、描述、影响、攻击场景、修复建议、代码引用。
- Low / Informational notes 使用简洁表格。
- 简洁的 checklist / pattern 类别级覆盖摘要。
- Limitations and Assumptions。

## 关键资源

| 资源 | 路径 | 用途 |
|------|------|------|
| 审计报告模板 | `resources/audit_report_template.md` | 最终报告结构 |
| 漏洞模式 | `references/VULNERABILITY_PATTERNS.md` | 漏洞检查模式 |
| DeFi 检查清单 | `references/DEFI_CHECKLIST.md` | 协议特定检查 |

## 使用原则

必须做：

- 按 Phase 0 到 Final 顺序执行。
- 每个阶段用不同重点重新读代码。
- Phase 2 必须完成足够理解后才能进入漏洞发现。
- Phase 3 必须结合函数级和流程级分析。
- Phase 4 必须验证候选漏洞，避免把误报写进报告。
- 最终保存 `.audit/Audit-Report.md`。

不要做：

- 不要输出完整中间阶段报告。
- 不要保存 `.audit/blueprints/`。
- 不要在聊天中粘贴完整函数清单。
- 不要输出完整 checklist pass/fail 表。
- 不要输出工具原始日志或 JSON。
- 不要展开 Phase 统计、验证分类统计、重读效率统计。
- 不要为大量 Low / Informational 问题逐个写完整 finding。
- 不要把自动化工具结果未经确认直接当作 finding。
- 不要默认全项目跑 Mythril。

## 为什么更省 Token

1. **减少 output token**：中间 Phase 不再输出大段 markdown。
2. **减少重复 input token**：后续阶段不再读取 `.audit/blueprints/*.md`。
3. **减少冗余报告内容**：最终报告只保留确认后的漏洞和必要摘要。
4. **减少低价值表格**：不再输出 full inventory、完整 pass/fail、raw tool output。

## 调整建议

- 新增漏洞模式：编辑 `references/VULNERABILITY_PATTERNS.md`。
- 新增协议检查：编辑 `references/DEFI_CHECKLIST.md`。
- 调整最终报告格式：编辑 `resources/audit_report_template.md`。
- 想更轻量：限制审计范围到核心合约，跳过 Phase 1 重工具。
- 想更严格：增加测试、PoC、或针对高危 finding 做定向 symbolic execution。

## 总结

v2.10.0 的核心变化是：**多阶段审计仍然保留，但中间产物不再写文件、不再完整输出，最终只生成 `.audit/Audit-Report.md`，并默认省略 full inventory、完整 checklist、raw tool output 和冗长验证细节。**
