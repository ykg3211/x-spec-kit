<!--
Sync Impact Report:
Version change: 1.0.0 → 1.1.0 (Minor update)
Modified principles: 仓库架构、技术栈规范、组件库层次结构、API规范、写法规范
Added sections: 包结构概览、包详细说明
Removed sections: N/A
Templates requiring updates: N/A (Standalone frontend constitution)
Follow-up TODOs: None
-->

# Cozeloop 前端开发工程规范宪法

## 项目概述

本宪法规范 Cozeloop 前端开发的核心原则和标准，确保代码质量、架构一致性和开发效率。

## 仓库架构

前端仓库是 Rush.js 管理的 Monorepo，前端项目包名和相对目录位置都可以从 **rush.json** 中查询（**不要读取** pnpm-lock.yaml）。

### 包结构概览

```bash
frontend/apps/cozeloop          # Cozeloop 应用入口
frontend/packages/cozeloop
├── account/                    # 账户管理
├── api-schema/                 # API 接口包
├── auth-pages/                 # 登录/认证页面
├── base-hooks/                 # 基础 Hooks
├── biz-components/             # 业务组件
├── biz-hooks/                  # 业务 Hooks
├── components/                 # 通用组件
├── env/                        # 环境配置
├── evaluate/                   # 评测核心功能
├── evaluate-components/        # 评测组件
├── evaluate-pages/             # 评测页面
├── guard/                      # 权限守卫
├── i18n/                       # 国际化
├── intl/                       # 国际化运行时
├── observation/                # 观测功能
│   ├── trace-detail/           # Trace 详情
│   ├── trace-list/             # Trace 列表
│   └── trace-struct-data/      # Trace 结构数据
├── observation-pages/          # 观测页面
├── prompt-components/          # Prompt 组件
├── prompt-pages/               # Prompt 页面
├── rsbuild-config/             # Rsbuild 构建配置
├── stores/                     # 通用全局 store
├── tailwind-config/            # Tailwind 配置
├── tailwind-plugin/            # Tailwind 插件
├── tea/                        # TEA 适配器
└── toolkit/                    # 工具库
```

### 包详细说明

#### 核心基础包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/account | CozeLoop 账户管理 | `@cozeloop/api-schema`, `ahooks`, `zustand` | 提供用户账户相关的状态管理和业务逻辑 |
| @cozeloop/api-schema | API 接口定义和类型声明 | `@coze-arch/idl2ts-runtime`, `eventemitter3` | 统一管理所有 API 接口定义，提供类型安全的接口调用。导出模块：`observation`, `evaluation`, `data`, `llm-manage`, `foundation`, `prompt` |
| @cozeloop/stores | 全局状态管理 | `zustand`, `immer`, `eventemitter3` | 提供基于 Zustand 的状态管理解决方案 |
| @cozeloop/toolkit | 通用工具库 | `dayjs` | 提供项目中常用的工具函数和实用程序 |

#### Hooks 相关包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/base-hooks | 基础 React Hooks | `@cozeloop/stores`, `zustand` | 提供项目中常用的基础 Hooks |
| @cozeloop/biz-hooks-adapter | 业务逻辑 Hooks | `@cozeloop/account`, `@cozeloop/api-schema`, `ahooks`, `zustand` | 封装业务相关的 Hooks，如数据获取、状态管理等 |

#### 组件相关包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/components | 通用 UI 组件库 | `@coze-arch/coze-design`, `@monaco-editor/react`, `react-sortable-hoc`, `lodash-es`, `nanoid`, `immer` | 提供项目中复用的通用组件 |
| @cozeloop/biz-components-adapter | 业务组件适配器 | `@coze-arch/coze-design`, `@cozeloop/components`, `@cozeloop/biz-hooks-adapter` | 封装业务相关的复合组件 |

#### 评测相关包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/evaluate | 评测功能核心包 | `@cozeloop/evaluate-components`, `@cozeloop/prompt-components`, `@visactor/vchart`, `react-error-boundary` | 提供完整的评测功能，包括实验管理、结果分析等 |
| @cozeloop/evaluate-components | 评测相关的 UI 组件 | `@dnd-kit/core`, `@monaco-editor/react`, `@textea/json-viewer`, `@visactor/vchart`, `papaparse` | 提供评测功能所需的专用组件 |
| @cozeloop/evaluate-pages | 评测功能页面 | `@cozeloop/evaluate`, `@cozeloop/evaluate-components` | 组装评测功能的完整页面 |

#### Prompt 相关包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/prompt-components | Prompt 编辑和管理组件 | `@codemirror/*`, `@coze-editor/editor`, `shiki`, `sortablejs` | 提供 Prompt 编辑、预览、管理等功能组件 |
| @cozeloop/prompt-pages | Prompt 功能页面 | `@cozeloop/prompt-components`, `@cozeloop/observation-component-adapter` | 组装 Prompt 功能的完整页面 |

#### 观测相关包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| observation/ | 观测功能模块 | - | 提供系统观测和链路追踪功能。包含子模块：`trace-detail/`, `trace-list/`, `trace-struct-data/` |
| @cozeloop/observation-pages | 观测功能页面 | `@cozeloop/trace-list` | 组装观测功能的完整页面 |

#### 认证和权限包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/auth-pages | 登录相关页面 | `@coze-arch/coze-design`, `@cozeloop/account`, `@cozeloop/i18n-adapter` | 提供登录、注册等认证页面 |
| @cozeloop/guard | 权限守卫组件 | `@cozeloop/biz-hooks-adapter` | 提供路由和组件级别的权限控制 |

#### 国际化包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/i18n-adapter | 国际化适配器 | `@coze-arch/i18n`, `@coze-studio/loop-i18n-resource-adapter`, `i18next-browser-languagedetector` | 提供多语言支持和本地化功能 |

#### 配置和工具包

| 包名 | 功能描述 | 主要依赖 | 作用说明 |
|------|----------|----------|----------|
| @cozeloop/env-adapter | 环境配置适配器 | - | 处理不同环境下的配置管理 |
| @cozeloop/rsbuild-config | Rsbuild 构建配置 | `@rsbuild/core`, `@rsbuild/plugin-*`, `tailwindcss` | 提供统一的构建配置 |
| @cozeloop/tailwind-config | Tailwind CSS 配置 | `@cozeloop/tailwind-plugin`, `tailwindcss` | 提供项目的 Tailwind 配置 |
| @cozeloop/tailwind-plugin | 自定义 Tailwind 插件 | `@tailwindcss/forms`, `@tailwindcss/nesting`, `tailwindcss` | 扩展 Tailwind 功能，提供项目特定的样式工具 |
| @cozeloop/tea-adapter | TEA 适配器 | - | 提供与 TEA 框架的集成适配 |

## 技术栈规范

### 基础框架

- **React**: 18.2.0
- **TypeScript**: 5.8.2

### 构建工具

- **rsbuild**: 仅 App 应用入口项目需要构建，仓库内 workspace 依赖都是源码依赖无需构建

### 组件样式

- **样式方案**: Tailwind 或 Less module，尽量避免行内 style
- **样式组合**: 使用 classnames 组合多个样式类名

### 状态管理

- **跨组件状态**: Zustand (^4.4.7)
- **单组件状态**: React useState

### 单测工具

- **测试框架**: Vitest
- **测试策略**: 通常组件库/页面包不需要单测

### Hooks 库

- **官方库**: ahooks (^3.7.8)

### 组件库层次结构 (NON-NEGOTIABLE)

MUST 遵循三层组件库架构，优先考虑复用业务组件库 > 基础组件库：

1. **基础组件库**: @coze-arch/coze-design
2. **业务基础组件库**: @cozeloop/components  
3. **业务特性组件库**: @cozeloop/biz-components-adapter

**FORBIDDEN**: 添加其他组件库，重复创建新组件

## 命名规范

### 包名规范

- **新增包名**: 前缀为 @cozeloop，例如 @cozeloop/xxx

### 文件/文件夹名规范

- **格式**: 小写英文 + 中划线
- **示例**: some-dir, some-component.ts

### 组件名规范

- **语义要求**: 新增组件应该符合业务的语义规范，保持完整语义
- **FORBIDDEN**: 缩写或简写
- **一致性**: 组件的文件名和文件夹名规范保持一致

## API 规范 (NON-NEGOTIABLE)

### API 来源

- **MUST**: 前端 API 来源于 @cozeloop/api-schema
- **FORBIDDEN**: 直接 fetch url

### API Schema 管理

- **生成来源**: @cozeloop/api-schema 基于 idl 下的 thrift 定义生成
- **FORBIDDEN**: 直接修改此包下的任何文件
- **更新方式**: 在 @cozeloop/api-schema 目录下运行 `npm run update` 更新来源

### API 缺失处理

- **流程**: 如果没有发现可用的 API，需要和用户确认

## 写法规范

### React 导入规范 (NON-NEGOTIABLE)

- **FORBIDDEN**: `import React from 'react'`
- **MUST**: 使用具名导入，如 `import { useState, useEffect } from 'react'`

### 组件规模控制

- **MUST**: 单个 React 组件控制在 400 行以内
- **处理方式**: 超过则需要抽取组件和拆分

### 导入导出规范

- **推荐**: 尽量避免 export default，使用具名导入/导出

### 包管理原则

- **优先级**: 尽量避免新增包，在原有包中扩展功能

## 常用命令

### 依赖管理

```bash
# 更新依赖
rush update
```

### 代码质量检查

```bash
# ESLint 检查
npm run lint

# TypeScript 检查
rush ts-check -o xxx
```

## 开发流程规范

### 组件开发流程

1. **设计阶段**: 确认组件属于哪一层组件库
2. **实现阶段**: 遵循命名规范和代码规范
3. **测试阶段**: 根据组件类型决定是否需要单测
4. **集成阶段**: 确保 API 集成符合规范

### 代码审查要点

- [ ] 组件库层次结构正确
- [ ] 命名规范符合要求
- [ ] API 使用符合规范
- [ ] React 导入方式正确
- [ ] 组件规模在限制范围内

## 治理规范

### 宪法权威性

本宪法是 Cozeloop 前端开发的最高准则，所有开发活动必须遵循本宪法的规定。

### 修订流程

- 宪法修订需要团队评审和批准
- 版本号遵循语义化版本控制
- 重大变更需要提供迁移计划和文档

### 合规审查

- 所有 PR 必须验证是否符合本宪法原则
- 定期进行架构审查和规范合规检查

**版本**: 1.1.0 | **制定日期**: 2025-11-12 | **最后修订**: 2025-11-13
