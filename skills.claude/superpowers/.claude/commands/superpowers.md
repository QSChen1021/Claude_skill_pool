---
description: 启动 Superpowers 泛用式任务处理框架 — 系统化处理复杂任务、软件开发、方案设计与信息收集
---

# /superpowers — 泛用式任务处理框架

你正在使用 **Superpowers v2.0**：一个将 AI 代理转变为真正问题解决伙伴的完整框架，覆盖软件开发和各类复杂任务。

---

## 使用场景

**自动触发（复杂任务）**：
- "帮我创建 / 分析 / 设计 / 优化 / 解决 / 制定..."
- 涉及多个步骤或子任务
- 需要结构化输出（报告 / 文档 / 计划）

**不触发（简单任务）**：
- 单一信息查询
- 单步文件操作
- 直接答案问题

---

## 工作流入口

**非开发复杂任务** → 通用头脑风暴 → 任务拆解 → 信息收集 → 方案设计 → 分步执行

**开发任务** → Brainstorming → Design Spec → Writing Plans → TDD → Code Review → 完成分支

---

## 核心规则

- **开发任务**：必须先展示设计并获批，再写代码；严格 TDD；任务间必须代码审查
- **所有任务**：批量 > 循环；并行 > 串行；一次性完整输出 > 逐步提示
- **禁止**：无设计就实现；无失败测试就写生产代码；逐步返回可以批量完成的结果

---

## 输出规范

| 产物 | 路径 |
|---|---|
| 设计规范 | `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` |
| 实现计划 | `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md` |
| 分析报告 | `docs/superpowers/reports/YYYY-MM-DD-<topic>-analysis.md` |

执行前读取完整技能：`.claude/skills/superpowers/SKILL.md`

只有当用户拿到可执行的输出且所有适用规则已满足时，才算本次 `/superpowers` 完成。
