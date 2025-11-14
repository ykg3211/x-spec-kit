---

description: "Task list template for feature implementation"
---

# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: The examples below include test tasks. Tests are OPTIONAL - only include them if explicitly requested in the feature specification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [Story] Description`

- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: i18n 国际化文案

**Goal**: 添加所有 Code 评估器相关的中英文文案,支持国际化

- [ ] T001 Add Chinese i18n keys to xxxxx
- [ ] T002 Add English i18n keys to xxxxx
- [ ] T003 Update i18n type definitions if needed in xxxxxx

---

## Phase 2: API 类型定义

**Goal**: 更新前端的 TypeScript 类型定义

- [ ] T004 在 @cozeloop/api-schema 目录下运行 npm run update 更新 api schema
- [ ] T005 检索所有 spec 中的 api 是否已经存在。不存在则需要人工确认。

**Extra content start: 如果需要人工测试确认，则需要在此阶段加上以下相关内容**

### Manual Testing Path - Phase 2

1. Navigate to `packages/cozeloop/api-schema/src/prompt/`
2. Check UI

**⚠️ CHECKPOINT: Do not proceed to Phase 3 until Phase 2 is verified and approved**

**Extra content End**

---

## Phase 3: (User Story 1 : phase title)

**Goal**: {阶段一句话描述}

### Implementation for User Story 1

- [ ] T3.1 [US1] Create [Entity1] model in src/models/[entity1].py
- [ ] T3.2 [US1] Create [Entity2] model in src/models/[entity2].py
- [ ] T3.3 [US1] Implement [Service] in src/services/[service].py (depends on T3.1, T3.2)
- [ ] T3.4 [US1] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T3.5 [US1] Add validation and error handling
- [ ] T3.6 [US1] Add logging for user story 1 operations

**Extra content start: 如果需要人工测试确认，则需要在此阶段加上以下相关内容**

### Manual Testing Path - Phase 2

1. Navigate to `packages/cozeloop/api-schema/src/prompt/`
2. Check UI

**⚠️ CHECKPOINT: Do not proceed to Phase 3 until Phase 2 is verified and approved**

**Extra content End**
---

## Phase 4: (User Story 2 : phase title)

**Goal**: {阶段一句话描述}

### Tests for User Story 2 (OPTIONAL - only if tests requested) ⚠️

- [ ] T4.1 [US2] Contract test for [endpoint] in tests/contract/test_[name].py
- [ ] T4.2 [US2] Integration test for [user journey] in tests/integration/test_[name].py

### Implementation for User Story 2

- [ ] T4.3 [US2] Create [Entity] model in src/models/[entity].py
- [ ] T4.4 [US2] Implement [Service] in src/services/[service].py
- [ ] T4.5 [US2] Implement [endpoint/feature] in src/[location]/[file].py
- [ ] T4.6 [US2] Integrate with User Story 1 components (if needed)

**Extra content start: 如果需要人工测试确认，则需要在此阶段加上以下相关内容**

### Manual Testing Path - Phase 2

1. Navigate to `packages/cozeloop/api-schema/src/prompt/`
2. Check UI

**⚠️ CHECKPOINT: Do not proceed to Phase 3 until Phase 2 is verified and approved**

**Extra content End**

---

[Add more user story phases as needed, following the same pattern]

---

## Phase N: Final CheckPoints

**Goal**: Improvements that affect multiple user stories

- [ ] TN.1 检查 lint error
- [ ] TN.2 检查所有 phase & tasks 是否完成
