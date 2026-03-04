# React 前端工程治理 Skill - 核心指南

**版本:** v1.1.0  
**更新日期:** 2026-03  
**适用范围:** React 工程化前端项目（含 TS / 组件库 / CI 流程）

---

## 📋 目录

- 核心理念
- 治理流程
- Blueprint 目录规则
- 阶段说明
- 输出文件结构
- 风险模型
- 关键资源
- 使用指南
- 调整建议
- 核心设计决策

---

# 🎯 核心理念

## 1. 理解优先原则（强制）

在生成代码之前，必须先完成：

- 需求结构理解
- 组件边界确认
- 状态模型设计
- API 依赖分析
- 影响面评估

Phase 1（Technical Planning）是强制阶段。  
禁止直接进入开发阶段。

---

## 2. 分层分析模型

本 Skill 采用双层分析模型：

### A. Component-Based（组件级分析）

逐个组件分析：

- 职责边界
- Props 结构
- 状态来源
- 副作用来源
- 是否可复用
- 是否违反单一职责

### B. Flow-Based（业务流程级分析）

按用户操作流程分析：

- 页面加载
- 数据请求
- 状态变更
- 表单提交
- 异常处理
- 权限流转

用于发现：

- 状态不一致
- 副作用污染
- API 重复请求
- 隐性耦合

---

## 3. 文件化工作流（强制）

每个阶段必须：

- 独立输出完整文档
- 明确输入与输出
- 不允许跨阶段合并输出

### 标准目录结构

```
sun-frontend/
├── blueprints/
│   └── YYYY-MM-模块名/
│       ├── 1_Technical_Planning.md
│       ├── 2_Development_Design.md
│       ├── 3_Self_Test_Report.md
│       └── 4_PR_Gate_Report.md
└── Final-Delivery-Summary.md
```

---

## 4. 参考文件驱动机制

以下文件必须在对应阶段使用：

- references/react_ts_governance.md
- references/risk_level_definition.md
- references/scenario_matrix_guide.md

规则：

- Planning 阶段必须参考 governance
- Self-Test 阶段必须参考 scenario matrix
- PR Gate 阶段必须参考 risk level definition

---

## 5. 高效验证策略

PR Gate 阶段采用三级风险验证模型：

| Category | 含义 |
|----------|------|
| A | 逻辑确认，无需重新分析 |
| B | 需要定向代码复查 |
| C | 需要重新进行状态流重构分析 |

避免重复分析整个项目。

---

# 🔄 治理流程

整体流程：

Phase 1 技术方案（强制）  
→ Phase 2 开发设计  
→ Phase 3 自测验证  
→ Phase 4 PR Gate 审核  
→ Final 交付总结  

---

# 📁 Blueprint 目录创建规则（强制执行）

为保证需求可追溯性与治理一致性，每一个新需求必须创建独立 Blueprint 子目录。

---

## 一、创建时机

当进入 Phase 1 之前必须：

- 检查是否存在本次需求对应目录
- 若不存在 → 创建目录
- 若存在 → 禁止覆盖历史内容

---

## 二、命名规范

统一格式：

```
YYYY-MM-模块名
```

示例：

```
2026-03-OrderModule
2026-03-UserCenter
2026-04-CheckoutFlow
```

---

## 三、同月多次迭代

```
YYYY-MM-模块名-v2
YYYY-MM-模块名-v3
```

示例：

```
2026-03-OrderModule-v2
```

---

## 四、目录内部标准结构

每个需求目录必须包含：

```
1_Technical_Planning.md
2_Development_Design.md
3_Self_Test_Report.md
4_PR_Gate_Report.md
```

复杂需求可扩展：

```
api_contract.md
state_design.md
deployment_plan.md
```

---

## 五、治理约束

1. 禁止多个需求共用一个目录  
2. 禁止覆盖历史 Blueprint 文件  
3. 每个 PR 必须对应一个 Blueprint 目录  
4. Blueprint 目录作为治理审计依据，不可删除  

---

## 六、AI 执行责任

当触发新需求时，必须：

- 自动生成符合规范的目录名
- 输出完整四阶段文件结构
- 按阶段分别产出文档

否则视为流程未完成。

---

# 📘 Phase 1: 技术方案（强制）

## 目标

建立：

- 页面结构
- 组件层级
- 状态模型
- API 模型
- 复用策略
- 风险预判

---

## 必须完成

- 需求拆解
- 页面模块图
- 组件树
- 状态流设计
- API 依赖矩阵
- 异常路径设计
- 权限路径设计
- 组件库复用分析（@/sun-frontend/react-ui）
- 复杂度评估
- 风险等级初评

---

## 输出

`blueprints/YYYY-MM-模块名/1_Technical_Planning.md`

---

# 📘 Phase 2: 开发设计

## 目标

明确实现结构，而不是直接写代码。

---

## 必须完成

- 文件结构规划
- TS 类型定义设计
- Hooks 使用策略
- 副作用管理说明
- 是否抽象自定义 Hook
- 是否拆分子组件
- 依赖新增说明
- 性能风险评估

---

## 强制规则

- 禁止新增全局状态污染
- 禁止在 useEffect 中写非必要逻辑
- 禁止无依赖数组副作用
- 禁止隐式 any
- 必须遵守 ESLint hooks 规则

---

## 输出

`blueprints/YYYY-MM-模块名/2_Development_Design.md`

---

# 📘 Phase 3: 自测验证

## 目标

模拟 QA 与 Code Review。

---

## 必须完成

- 功能路径验证
- 边界值验证
- 异常路径验证
- 状态一致性验证
- Hook 依赖完整性验证
- 组件复用合理性验证
- 影响面评估
- 回归风险评估

---

## 必须参考

`references/scenario_matrix_guide.md`

---

## 输出

`blueprints/YYYY-MM-模块名/3_Self_Test_Report.md`

---

# 📘 Phase 4: PR Gate 审核

## 目标

结构化风险评估报告。

---

## 输出内容

- 风险等级（Low / Medium / High）
- 改动文件列表
- 潜在影响模块
- 是否需要专项测试
- 是否建议灰度发布
- 回滚难度评估

---

## 必须参考

`references/risk_level_definition.md`

---

## 输出

`blueprints/YYYY-MM-模块名/4_PR_Gate_Report.md`

---

# 📤 最终交付总结

必须包含：

- 本次需求摘要
- 复杂度等级
- 风险等级
- 主要变更点
- 影响范围
- 回归建议

输出：

`Final-Delivery-Summary.md`

---

# 📊 风险模型

风险等级由以下维度综合评估：

- 影响范围
- 状态改动范围
- API 改动范围
- 是否修改公共组件
- 是否修改基础 Hook

详见：

`references/risk_level_definition.md`

---

# 📚 关键资源

| 资源 | 路径 | 用途 |
|------|------|------|
| 技术方案模板 | resource/technical_plan_template.md | Phase 1 使用 |
| 自测报告模板 | resource/self_test_report_template.md | Phase 3 使用 |
| PR Gate 模板 | resource/pr_gate_output_template.md | Phase 4 使用 |
| React TS 治理规范 | references/react_ts_governance.md | 规范基线 |
| 风险等级定义 | references/risk_level_definition.md | 风险模型 |
| 场景矩阵指南 | references/scenario_matrix_guide.md | 自测矩阵 |

---

# 🚀 使用指南

## 执行顺序（必须）

1. 创建 Blueprint 目录  
2. 完成 Phase 1 并输出  
3. 完成 Phase 2 并输出  
4. 完成 Phase 3 并输出  
5. 完成 Phase 4 并输出  
6. 生成最终总结  

---

## DO

- 一个阶段完全完成再进入下一个
- 每个阶段独立输出
- 每阶段重新分析代码
- 使用模板文件

---

## DON'T

- 不允许跳过 Phase 1
- 不允许直接写代码
- 不允许合并多个阶段输出
- 不允许忽略风险等级评估

---

# 🔧 调整建议

添加新的技术栈支持，修改：

- `references/react_ts_governance.md`
- `resource/technical_plan_template.md`

增强风险模型，修改：

- `references/risk_level_definition.md`

增强测试矩阵，修改：

- `references/scenario_matrix_guide.md`

---

# 💡 核心设计决策

## 为什么技术方案是强制？

避免：

- 组件结构混乱
- 状态提升错误
- 副作用污染
- 代码返工

---

## 为什么必须文件化？

避免：

- 上下文丢失
- 阶段混乱
- 难以追溯

---

## 为什么分四阶段？

因为：

- 方案设计
- 实现设计
- 验证
- 风险评估

属于不同认知模型。

---

# 📝 总结

本 Skill 核心：

- 强制技术方案
- 双层分析模型
- Blueprint 目录治理
- 文件化工作流
- 风险等级控制
- 标准化输出

目标不是生成代码。  
目标是：

建立 React 前端工程的 AI 治理层。