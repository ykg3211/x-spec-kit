---
description: Execute the implementation planning workflow using the plan template to generate design artifacts.
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. **设置**：从仓库根目录运行 `{SCRIPT}` 并解析 JSON 以获取 FEATURE_SPEC、IMPL_PLAN、SPECS_DIR、BRANCH。对于参数中的单引号，如 "I'm Groot"，请使用转义语法：例如 'I'\''m Groot'（如果可能，也可以使用双引号："I'm Groot"）。

2. **加载上下文**：读取 FEATURE_SPEC 和 `/memory/constitution.md`。加载 IMPL_PLAN 模板（已复制）。

3. **执行计划工作流**：遵循 IMPL_PLAN 模板中的结构来：
   - 填写技术背景（将未知项标记为“需要澄清”）
   - 从章程中填写章程检查部分
   - 评估关卡（如果违规行为不合理，则报错）
   - 阶段 0：解决所有“需要澄清”项，并在技术分析部分记录决策
   - 阶段 1：完成在 plan.md 的相应部分设计
   - 阶段 1：通过运行代理脚本更新代理上下文
   - 设计后重新评估章程检查

4. **停止并报告**：命令在阶段 1 规划后结束。报告分支和 IMPL_PLAN 路径。所有设计细节都内联记录在 plan.md 中。

## 阶段

### 阶段 0：技术分析与决策

1. **从上述技术背景中提取未知项**：
   - 对于每个“需要澄清”项 → 进行研究并做出决策
   - 对于每个依赖项 → 确定最佳实践
   - 对于每个集成 → 记录模式

2. **研究并解决未知项**：

   ```text
   对于技术背景中的每个未知项：
     任务：“为 {功能背景} 研究 {未知项}”
   对于每个技术选择：
     任务：“寻找 {技术} 在 {领域} 中的最佳实践”
   ```

3. **直接在 plan.md 中记录发现**：
   - 添加到“技术分析”部分
   - 为每个决策记录以下内容：
     - 决策：[选择了什么]
     - 理由：[为什么选择]
     - 考虑过的替代方案：[评估了哪些其他方案]

**输出**：所有技术决策都内联记录在 plan.md 中

### 阶段 1：设计与架构

**先决条件**：所有技术决策都已记录在技术分析部分

1. **从功能规格中提取 i18n 文案** → 在 plan.md 中记录：
   - 所有提及的 i18n 文案
   - 按照需求类型寻找在修改的文件路径
   - 修改完成之后需要同步更新相应的 type 文件

2. **通过命令更新 api schema 并且校验，并且需要有一份 mock server 用于前端前置开发** → 在 plan.md 中记录：
   - 需要检验生成的定义是否符合 spec 或者 api doc 中的要求
   - 必要时，需要实现 mock server 用于前端前置开发。因为后端实际接口可能还未完成。

3. **组件拆分 & 实现** → 在 plan.md 中记录：
   - 组件拆分策略
   - 每个组件的实现细节
   - 组件之间的交互
   - 提供一个 components-preview page 用于展示组件的交互和功能

3. **记录集成场景** → 添加到 plan.md：
   - 来自用户故事的测试场景
   - 集成路径和依赖关系

4. **代理上下文更新**：
   - 运行 `{AGENT_SCRIPT}`
   - 这些脚本会检测正在使用的 AI 代理
   - 更新相应的特定于代理的上下文文件
   - 仅添加当前计划中的新技术
   - 保留标记之间的手动添加内容

**输出**：完整的 plan.md，其中包含所有设计细节（数据模型、API 合约、测试场景）的内联记录

## 关键规则

- 使用绝对路径
- 在关卡失败或有未解决的澄清项时报错