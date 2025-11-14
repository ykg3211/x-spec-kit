---
description: Execute the implementation plan by processing and executing all tasks defined in tasks.md
  ps: .specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. Run `.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` from repo root and parse FEATURE_DIR and AVAILABLE_DOCS list. All paths must be absolute. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. **Check checklists status** (if FEATURE_DIR/checklists/ exists):
   - Scan all checklist files in the checklists/ directory
   - For each checklist, count:
     - Total items: All lines matching `- [ ]` or `- [X]` or `- [x]`
     - Completed items: Lines matching `- [X]` or `- [x]`
     - Incomplete items: Lines matching `- [ ]`
   - Create a status table:

     ```text
     | Checklist | Total | Completed | Incomplete | Status |
     |-----------|-------|-----------|------------|--------|
     | ux.md     | 12    | 12        | 0          | ✓ PASS |
     | test.md   | 8     | 5         | 3          | ✗ FAIL |
     | security.md | 6   | 6         | 0          | ✓ PASS |
     ```

   - Calculate overall status:
     - **PASS**: All checklists have 0 incomplete items
     - **FAIL**: One or more checklists have incomplete items

   - **If any checklist is incomplete**:
     - Display the table with incomplete item counts
     - **STOP** and ask: "Some checklists are incomplete. Do you want to proceed with implementation anyway? (yes/no)"
     - Wait for user response before continuing
     - If user says "no" or "wait" or "stop", halt execution
     - If user says "yes" or "proceed" or "continue", proceed to step 3

   - **If all checklists are complete**:
     - Display the table showing all checklists passed
     - Automatically proceed to step 3

3. Load and analyze the implementation context:
   - **REQUIRED**: Read tasks.md for the complete task list and execution plan
   - **REQUIRED**: Read plan.md for tech stack, architecture, and file structure
   - **IF EXISTS**: Read data-model.md for entities and relationships
   - **IF EXISTS**: Read contracts/ for API specifications and test requirements
   - **IF EXISTS**: Read research.md for technical decisions and constraints
   - **IF EXISTS**: Read quickstart.md for integration scenarios

4. **Project Setup Verification**:
   - **REQUIRED**: Create/verify ignore files based on actual project setup:

   **Detection & Creation Logic**:
   - Check if the following command succeeds to determine if the repository is a git repo (create/verify .gitignore if so):

     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```

   - Check if Dockerfile* exists or Docker in plan.md → create/verify .dockerignore
   - Check if .eslintrc*or eslint.config.* exists → create/verify .eslintignore
   - Check if .prettierrc* exists → create/verify .prettierignore
   - Check if .npmrc or package.json exists → create/verify .npmignore (if publishing)
   - Check if terraform files (*.tf) exist → create/verify .terraformignore
   - Check if .helmignore needed (helm charts present) → create/verify .helmignore

   **If ignore file already exists**: Verify it contains essential patterns, append missing critical patterns only
   **If ignore file missing**: Create with full pattern set for detected technology

   **Common Patterns by Technology** (from plan.md tech stack):
   - **Node.js/JavaScript/TypeScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`

   **Tool-Specific Patterns**:
   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`

5. Parse tasks.md structure and extract:
   - **Task phases**: i18n, api, api mock server, 组件/模块/hooks 的使用和封装部分（可以不止一个）, 按照 user story 进行集成, Review & Lint
   - **Execution flow**: Order and dependency requirements

6. Execute implementation following the task plan:
   - **Phase-by-phase execution**: Complete each tasks before moving to the next phase
   - **Task-by-task execution**: 在一个阶段中，你必须要按照 tasks.md 中定义的顺序，一个一个 task 进行实现。不允许跳过任何 task，也不允许并行实现多个 task。tasks 可以直接当成 todo 来依次实现
   - **Phase manual confirmation**: 如果该 phase 最后需要人工测试确认，那么在该 phase 结束后，需要询问用户是否确认该 phase 完成。在人工确认之前，不允许继续执行下一个 phase！
   - **File-based coordination**: Tasks affecting the same files must run sequentially
   - **Validation checkpoints**: Verify each phase completion before proceeding

7. Implementation execution rules:
   - **Task execution rules**: 在执行一个新的 phase 之前，先判断该 phase 的 title 里是否包含类似 `[US1]` 这种标记。如果有，则必须要需要使用 `find-related-context` 这个 subagent 从 spec 中搜寻该标记位相关的上下文内容，回填到上下文中。
   - **Integration work**: 使用已经开发完成的 api/组件/hooks/模块 按照 user story 进行集成
   - 不允许自己生成 todo，可以直接将这个 phase 中的 tasks 直接当成 todo 来依次实现！
   - 如果涉及文案的改动/变更，在实现之前需要先调用 `find-related-context` 这个 subagent 从 spec 中搜寻相关的文案内容。
     - 随后只能使用 spec 中搜寻到的文案内容，不允许自己生成文案！
     - 如果发现文案不合适的情况，及时询问用户确认如何处理！千万不允许自己生成文案！

8. Phase 完成之后 review tasks：
   - 在完成一个 phase 所有的任务之后，进入人工确认之前，需要进行这步确认。
   - 调用 `task-completion-validator` 这个 subagent 来验证该 phase 中是否有 tasks 可能未实现。
     - 带着该 phase 的 title 和具体的 tasks 原文列表，并且带上当前的实现上下文，来调用 `task-completion-validator` 这个 subagent。
     - 上下文包括但不限于：当前变更的文件列表（不包含代码diff），相关联的 spec user storey 内容，整篇 plan.md 内容。
   - 如果有未实现的 tasks，需要询问用户是否确认是否需要重新实现这些 tasks。
   - 如果用户确认需要重新实现这些 tasks，则需要重新实现这些 tasks。
   - 如果用户确认不需要重新实现这些 tasks，则可以直接进入人工确认阶段。

Note: This command assumes a complete task breakdown exists in tasks.md. If tasks are incomplete or missing, suggest running `/speckit.tasks` first to regenerate the task list.

