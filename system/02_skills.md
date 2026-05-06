# Skill 总览

## 什么是 Skill

Skill 是一个**可复用的操作协议**。它定义了：
- **Purpose**：这个 Skill 解决什么问题
- **Input**：需要什么前置材料
- **Output**：产生什么文件/结果
- **Process**：具体的步骤流程
- **Interaction**：与我（用户）的交互模式

每个 Skill 对应一个独立的 `.md` 文件，存放在 `skills/` 目录下。

## 市场参考文档

- `templates/tomato_system_market_essence.md` — 番茄系统流脑洞爽文市场精华数据
- 每次写作任务启动前，Orchestrator 应自动参考此文档

## 7 个 Skill 一览

| # | Skill | Agent | 核心产出 | 交互模式 |
|---|-------|-------|---------|---------|
| 1 | `world-building` | World Architect | `bible/*.md` | 问答 → 输出 → 确认 |
| 2 | `character-design` | Character Designer | `characters/*.md` | 模板填充 → 扩展 |
| 3 | `outline-planner` | Plot Planner | `outline/*.md` | 粗纲 → 细纲 → 锁定 |
| 4 | `chapter-writing` | Writer | `chapters/*.md` | 逐章输出，每5章审阅 |
| 5 | `editor` | Editor | 修改后章节 | 批量编辑 |
| 6 | `consistency-check` | Consistency Checker | 审查报告 | 自动调用 DeepSeek |
| 7 | `market-analysis` | Market Analyst | 分析报告 | 基于内置数据 + 用户对标清单 |

## Skill 调用流程（通用）

- 用户指令
  - → Orchestrator 判断：需要哪个 Skill？
    - → 读取 skills/ 中对应的 Skill 定义文件
      - → 按 Skill 定义的 Process 执行
        - → 每步完成后向用户汇报，需要决策时提问
          - → 产出写入 output/ 对应目录
            - → 更新 meta/project_status.md

## Skill 之间的依赖关系

- `world-building` → `character-design` → `outline-planner` → `chapter-writing` ─→ `editor`
  - `chapter-writing` → `consistency-check` → `market-analysis`

- `world-building`、`character-design`、`outline-planner`：写作**准备阶段**，可并行但建议顺序
- `chapter-writing`：**核心生产阶段**，依赖前三者完成
- `editor`、`consistency-check`：**质量阶段**，依赖正文完成
- `market-analysis`：**独立阶段**，在 Phase 0 和正文开始后各执行一次

## 调用 DeepSeek 的时机

| Skill 内 | 说明 |
|----------|------|
| `consistency-check` | 主要使用场景，全文级审查 |
| `chapter-writing` | 每章完成后自动生成摘要（可选） |
| `outline-planner` | 审查大纲逻辑完整性（可选） |

| 独立触发 | 说明 |
|----------|------|
| 每 10 章 | 全量一致性检查 |
| 每卷完成 | 深度审查 + 伏笔回收追踪 |
| 用户主动要求 | 随时可以触发 |
