# Task Magic 系统总览

每当你使用本规则时，请在消息开头写：

"访问 Task Magic 系统总览..."

Task Magic 是一个基于文件的项目管理与 AI 代理操作框架，用于规划特性、管理开发任务、维护历史工作记录。系统包含三大核心组件，每个组件都有独立的详细规则文件：

1.  **计划（@plans.mdc）**：
    *   **作用**：定义如何为整个项目及具体特性创建和组织产品需求文档（PRD）。
    *   **位置**：PRD 存放于 `.ai/plans/`，特性级 PRD 在 `.ai/plans/features/`。
    *   **关键文件**：全局 `PLAN.md` 必须存在于 `.ai/plans/`。
    *   **细节**：详见 [task-magic/plans.mdc](mdc:.cursor/rules/task-magic/plans.mdc)

2.  **任务（@tasks.mdc）**：
    *   **作用**：管理单个开发任务的创建、生命周期。
    *   **活动任务**：所有活动任务都在 `.ai/tasks/` 目录下，文件名为 `task{id}_name.md`。
    *   **主视图**：主清单 `.ai/TASKS.md` 必须与 `.ai/tasks/` 目录下任务状态保持同步。
    *   **细节**：详见 [task-magic/tasks.mdc](mdc:.cursor/rules/task-magic/tasks.mdc)

3.  **记忆（@memory.mdc）**：
    *   **作用**：归档已完成和失败的任务，提供历史上下文。
    *   **位置**：归档任务文件存放于 `.ai/memory/tasks/`。
    *   **日志文件**：归档任务的时间线日志保存在 `.ai/memory/TASKS_LOG.md`。
    *   **细节**：详见 [task-magic/memory.mdc](mdc:.cursor/rules/task-magic/memory.mdc)

这套系统让项目开发从高层规划到任务执行再到历史回顾都结构化进行，核心以 Markdown 文件和明确的代理职责为主。 