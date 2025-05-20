# AI 记忆系统

每当你使用本规则时，请在消息开头写：

"检查 Task Magic 记忆..."

本项目在 `.ai/memory/` 目录下维护一套记忆系统，用于存储已完成、失败或被替代的工作历史，为后续开发提供宝贵上下文。归档内容包括任务和计划。

## 结构

记忆系统包括：

### 任务归档

*   **`.ai/memory/tasks/`**：存放已归档任务的完整 Markdown 文件（`task{id}_name.md`），这些任务状态为 `completed` 或 `failed`，保留原始细节、描述和测试策略。
*   **`.ai/memory/TASKS_LOG.md`**：仅追加的 Markdown 日志，按时间记录任务归档。每条记录包括任务 ID、标题、最终状态、依赖、归档时的描述。

### 计划归档

*   **`.ai/memory/plans/`**：存放已归档 PRD（包括全局 `PLAN.md` 和特性计划如 `{feature}-plan.md`）。计划归档场景包括：全部特性已实现且稳定、废弃、被新版本替代。
*   **`.ai/memory/PLANS_LOG.md`**：仅追加的 Markdown 日志，按时间记录计划归档。每条记录应包含归档计划在 `.ai/memory/plans/` 下的完整路径、版本或日期、归档原因（如已完成、废弃、替代）、简要描述或标题。
    *   **日志格式示例：**
        ```markdown
        - **归档计划:** `.ai/memory/plans/old-feature-plan.md`
          - **归档时间:** YYYY-MM-DD HH:MM:SS
          - **原因:** 废弃
          - **标题:** PRD: Old Feature
          - **原文件（可选）:** {.ai/plans/features/old-feature-plan.md}
        ```

## 目录与文件管理

操作记忆系统时，代理应始终先检查所需目录和文件是否存在：

1. **目录检查**：操作前用 `list_dir` 或 `file_search` 检查 `.ai/memory/tasks/`、`.ai/memory/plans/` 是否存在。不存在时，`edit_file` 写入时会自动创建父目录。
2. **文件检查**：操作 `.ai/memory/TASKS_LOG.md`、`.ai/memory/PLANS_LOG.md` 前，用 `file_search` 检查文件是否存在。
3. **安全创建/修改文件**：如日志文件不存在且需初始化（如 `# Task Archive Log\n\n`），用 `edit_file` 创建。追加内容时，先 `read_file` 读取现有内容，拼接后再 `edit_file` 写回。
4. **归档计划文件**：移动计划文件（如从 `.ai/plans/features/` 到 `.ai/memory/plans/`），必须用 `run_terminal_cmd` 执行 `mv`，不要用 `edit_file`+`delete_file`，以保证原子性和版本历史。

## 用途与使用

记忆系统是项目开发和规划的历史记录。

**何时查阅记忆：**

*   **了解历史实现/规划**：新任务或新特性规划前，查阅记忆（`TASKS_LOG.md`、`PLANS_LOG.md` 及归档文件）了解类似或相关特性如何实现。
*   **避免重复劳动**：检查类似任务、需求或计划是否已被处理。
*   **规划相关特性**：查阅历史任务和计划，辅助新特性拆解。
*   **排查失败任务**：如任务曾失败，查阅归档文件（含 YAML 的 `error_log`）获取原因。
*   **决策历史背景**：归档计划反映某一时点的项目目标、需求和方向。

**如何查阅记忆：**

1.  **先看日志**：阅读 `.ai/memory/TASKS_LOG.md` 和 `.ai/memory/PLANS_LOG.md`，按时间了解归档内容，筛选标题、描述、归档原因相关的条目。
2.  **深入归档文件**：如日志条目高度相关，进一步阅读 `.ai/memory/tasks/` 或 `.ai/memory/plans/` 下的完整归档文件。

善用历史上下文，AI 能更高效决策、保持一致性、提升项目效率。 