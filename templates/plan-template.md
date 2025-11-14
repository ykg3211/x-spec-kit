# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

<!-- Extract from feature spec: primary requirement + technical approach from research, specify frontend -->

[High-level description of the frontend feature and its technical approach extracted from requirements]

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: React 18.2.0 + TypeScript 5.8.2
**Primary Dependencies**: @coze-arch/coze-design, @cozeloop/components, @cozeloop/biz-components-adapter, Zustand ^4.4.7, ahooks ^3.7.8
**Build Tool**: rsbuild (仅 App 应用入口项目需要构建)
**Testing**: Vitest (组件库/页面包通常不需要单测)
**Target Platform**: Web Browser (Rush.js Monorepo)
**Project Type**: Frontend Web Application
**Performance Goals**: Core Web Vitals compliance, bundle size optimization
**Constraints**: 单个组件 <400 行, 遵循三层组件库架构, 禁止直接 fetch URL
**Scale/Scope**: [domain-specific, e.g., 10k users, 50 screens or NEEDS CLARIFICATION]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

<!-- Explain how the design follows documented technical patterns and standards -->

**Frontend Constitution Compliance** (based on `frontend-constitution.md`):

- [ ] **组件库层次结构**: 遵循三层架构 (@coze-arch/coze-design → @cozeloop/components → @cozeloop/biz-components-adapter)
- [ ] **API 规范**: 使用 @cozeloop/api-schema，禁止直接 fetch URL
- [ ] **React 导入规范**: 使用具名导入，禁止 `import React from 'react'`
- [ ] **组件规模控制**: 单个组件控制在 400 行以内
- [ ] **命名规范**: 包名使用 @cozeloop 前缀，文件名使用小写英文+中划线
- [ ] **技术栈合规**: React 18.2.0, TypeScript 5.8.2, Zustand, ahooks
- [ ] **样式规范**: 使用 Tailwind 或 Less module，避免行内 style

**Complexity Justification Required If**:

- 添加三层组件库架构之外的组件库
- 直接 fetch URL 而不使用 @cozeloop/api-schema
- 创建新包而不是扩展现有包
- 组件超过 400 行而不进行拆分
- 使用 `import React from 'react'` 默认导入
- 绕过既定的测试要求

## Technical Analysis

<!-- Technical analysis and design decisions -->

[Detailed technical analysis including architecture design, technology selection, and implementation approach]

### Related API and IDL

<!-- Related API interfaces and data structures -->

- **[API Name]**: [Purpose and interface description]
- **[Data Format]**: [Request/response data structure]

```typescript
// API type definition example from @cozeloop/api-schema
interface FeatureAPI {
  // Define related API interface types
}
```

### Packages to Change

<!-- Packages and modules that need to be modified -->

- **@cozeloop/components**: [Add feature-related business basic components]
- **@cozeloop/biz-components-adapter**: [Add feature-specific business components]
- **@cozeloop/api-schema**: [Update API schema via `npm run update` - DO NOT modify files directly]
- **frontend/apps/cozeloop**: [Add new pages and routes to main application]

### Components to Change

<!-- Business components to add / modify. REQUIRED: list components' props, which components to reuse -->

#### 基础组件层 (@coze-arch/coze-design)

- **复用组件**: [List existing basic components to reuse]

#### 业务基础组件层 (@cozeloop/components)

- **新增组件**: [List new business basic components to create]
- **修改组件**: [List existing components to modify]

#### 业务特性组件层 (@cozeloop/biz-components-adapter)

- **新增组件**: [List new feature-specific components to create]
- **组件规模**: [Ensure each component < 400 lines]

#### Components Preview Page

<!-- Add feature-related business components to preview page -->

- **preview page**: [增加一个路由页面专门用于渲染本批次新增和变更的组件]
- **[Component Name]**: [Component description]
  - **Props**: [List of component props and their types]
  - **Usage**: [Example of component usage in preview page]

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
└── tasks.md             # Tasks by plan (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (Rush.js Monorepo Structure)

```text
frontend/
├── apps/
│   └── cozeloop/            # Main application entry
│       ├── src/
│       │   ├── pages/       # New feature pages
│       │   └── routes/      # Route configuration
│       └── package.json
├── packages/
│   ├── cozeloop/
│   │   ├── components/      # Business basic components
│   │   │   └── [feature-name]/
│   │   ├── biz-components-adapter/  # Feature-specific components
│   │   │   └── [feature-name]/
│   │   └── api-schema/      # API type definitions (auto-generated)
│   └── arch/
│       └── coze-design/     # Basic UI components (reuse only)
└── config/                  # Shared configurations
    ├── eslint-config/
    ├── ts-config/
    └── vitest-config/
```

**Structure Decision**: Rush.js Monorepo with three-tier component library architecture

## Implement Phases

<!-- Each phase is testable -->

### Phase 1: i18n 国际化文案

<!-- Tasks for this phase -->

**Main Tasks**:

- 找到对应的 i18n 配置文件
- 增加中英文的文案到配置文件内
- 更新对应的 type 文件
- 保证编译通过

**Acceptance Criteria**:

- [ ] key 没有冲突，没有遗漏

### Phase 2: API 类型定义

<!-- Tasks for this phase -->

**Main Tasks**:

- 确认 API Schema 更新，在 @cozeloop/api-schema 运行 `npm run update`
- 确认所有 spec 中的 api 是否已经存在。不存在则需要人工确认。

**Acceptance Criteria**:

- [ ] API Schema 已更新且类型定义完整
- [ ] TypeScript 编译成功，无 `any` 类型
- [ ] 确认 api doc 中的 api 都在 api schema 中定义

### Phase 3: Api Mock Server （如果存在 API 相关计划工作， 则需要建立 mock server 用于测试）

<!-- Tasks for this phase -->

**Main Tasks**:

- 根据 api doc 中涉及定义的 api 接口，建立 mock server 用于测试。

**Acceptance Criteria**:

- [ ] Mock Data 需要严格按照 api schema 中的定义

### Phase 4: 组件拆分实现

<!-- Tasks for this phase -->

**Main Tasks**:

- 建立基础 UI 组件 (复用 @coze-arch/coze-design)
- 实现业务基础组件
  - 根据需求描述需要添加到对应的 package 中，不可以乱加
- 实现特性组件
  - 根据需求描述需要添加到对应的 package 中，不可以乱加
- 对每个组件制定一份合理的 props 定义
- 并且需要实现一个汇总的 preview page，用于渲染展示所有的组件。
  - eg： xxx/xxx/components-preview

**Acceptance Criteria**:

- [ ] 每个组件的 props 定义需要符合 typescript 规范
- [ ] 每个组件的实现需要符合 coze-design 规范
- [ ] 每个组件的实现需要符合 typescript 规范
- [ ] 实现了一个新的 preview page，有一个路由！

### Phase 5: 集成阶段

<!-- Tasks for this phase -->

**Main Tasks**:

- 完成 API 集成 (使用 @cozeloop/api-schema)
- 添加状态管理逻辑
- 实现全部的 user story

**Acceptance Criteria**:

- [ ] 用户可以完成主要操作流程
- [ ] 状态管理正常工作

## E2E Cases

<!-- Consider requirements document and technical implementation approach above, write verifiable end-to-end test cases -->

### Case 1: Primary User Workflow

**User Story**: As a [user role], I want to [perform action], so that [achieve goal]

**Scenario**:

- **Given** [the preconditions and system state]
- **When** [the user performs specific actions]
- **Then** [the system should produce expected results]

**Acceptance Criteria**:

- [ ] [Specific acceptance condition 1]
- [ ] [Specific acceptance condition 2]
- [ ] [Specific acceptance condition 3]

### Case 2: Error Handling Workflow

**User Story**: As a [user role], when I encounter [error condition], I expect [system handling approach], so that [user can understand and recover]

**Scenario**:

- **Given** [the error preconditions]
- **When** [error condition is triggered]
- **Then** [the system should provide appropriate error handling and user feedback]

**Acceptance Criteria**:

- [ ] Error messages are clear and understandable
- [ ] Recovery or solution options are provided
- [ ] System state remains stable

### Case 3: Cross-Device Compatibility

**User Story**: As a [mobile user], I want to use the same functionality on [different devices], so that [I get consistent user experience]

**Scenario**:

- **Given** [different device environments (mobile, desktop)]
- **When** [user performs the same actions on different devices]
- **Then** [functionality should behave consistently and interface should adapt properly]

**Acceptance Criteria**:

- [ ] Mobile functionality is complete and usable
- [ ] Desktop functionality is complete and usable
- [ ] Responsive layout adapts correctly
- [ ] Interaction experience remains consistent across devices

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 添加第四层组件库] | [current need] | [why 三层架构 insufficient] |
| [e.g., 直接 fetch API] | [specific problem] | [why @cozeloop/api-schema insufficient] |

## Frontend Constitution Compliance Summary

**Constitution Version**: 1.0.0
**Compliance Status**: [PASS/FAIL with justifications]

**Key Compliance Points**:

- ✅ 三层组件库架构遵循
- ✅ API 规范遵循 (@cozeloop/api-schema)
- ✅ React 导入规范 (具名导入)
- ✅ 组件规模控制 (<400 行)
- ✅ 命名规范 (@cozeloop 前缀, 小写+中划线)
- ✅ 技术栈合规
- ✅ 样式规范 (Tailwind/Less module)

**Commands for Quality Assurance**:

```bash
# 更新依赖
rush update

# ESLint 检查
npm run lint

# TypeScript 检查
rush ts-check -o [package-name]

# API Schema 更新
cd frontend/packages/cozeloop/api-schema && npm run update
