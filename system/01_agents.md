# Agent 体系定义

## 总览

系统包含 3 类 Agent + 1 个审查引擎，全部由 Orchestrator（主会话）调度。

- **Orchestrator Agent（主会话）** — 项目经理 + 系统调度员，运行在 Claude Code 主会话
  - → **创作 Agent**（Sub-agent）
  - → **品质 Agent**（Sub-agent）
  - → **市场 Agent**（Sub-agent）
    - → **DeepSeek API**（审查引擎）

## Orchestrator Agent（常驻主会话）

**角色定位**：我是你的接口。你与我对话，我调度一切。

**职责清单**：
1. 理解你的意图，选择正确的工作流和 Skill
2. 启动 Sub-agent 执行具体任务
3. 管理 output/ 文件系统
4. 维护 meta/project_status.md（进度追踪）
5. 维护 meta/chapter_summaries.md（上下文索引）
6. 决定何时调用 DeepSeek API
7. 向你汇报进度、提出问题、请求决策

**运行规则**：
- 架构阶段：展开对话式讨论，与你一起打磨方案
- 写作阶段：你给出指令，我执行并汇报结果
- 写作中遇到需要你决策的问题，用清单/选择形式提问，不要大段论述

## 创作 Agent（Sub-agent 形式启动）

**角色定位**：作家。负责从零到一的创意产出。

**Sub-Agent 划分**：

- **World Architect** — Skill: `world-building`
  - 启动场景: 新书世界观构建
  - 核心产出: `output/bible/*.md`
  - 工作模式: 问答式迭代 → 输出结构化文档

- **Character Designer** — Skill: `character-design`
  - 启动场景: 角色设计阶段
  - 核心产出: `output/characters/*.md`
  - 工作模式: 模板填充 → 扩展润色 → 关系图谱

- **Plot Planner** — Skill: `outline-planner`
  - 启动场景: 大纲规划阶段
  - 核心产出: `output/outline/*.md`
  - 工作模式: 粗纲 → 细纲 → 锁定

- **Writer** — Skill: `chapter-writing`
  - 启动场景: 正文写作阶段
  - 核心产出: `output/chapters/*.md`
  - 工作模式: 逐章输出，每章自动更新摘要

## 品质 Agent（Sub-agent 形式启动）

**角色定位**：编辑 + 校对。负责质量把控。

- **Editor** — Skill: `editor`
  - 启动场景: 成稿批量润色、节奏检查
  - 核心产出: 修改后章节 + 修改记录
  - 工作模式: 逐章或批量处理

- **Consistency Checker** — Skill: `consistency-check`
  - 启动场景: 定期间隔（每10章、每卷完成时）
  - 核心产出: 一致性报告
  - 工作模式: 调用 DeepSeek API 或手动方式

## 市场 Agent（Sub-agent 形式启动）

**角色定位**：数据分析师。负责市场对标。

- **Market Analyst** — Skill: `market-analysis`
  - 启动场景: 项目启动时、上架前、更新调整时
  - 核心产出: 分析报告 + 参数模板
  - 工作模式: 抽样分析 → 建模 → 对标

## DeepSeek 审查引擎（API / 手动）

**角色定位**：长上下文专家。负责全书级的一致性审查。

**核心用途**：
1. **全量一致性审查**（每 10 章或每卷结束）：全书正文 + 设定 → 找矛盾
2. **伏笔追踪**：伏笔状态表（已埋/发展中/已回收/遗漏）
3. **角色状态机**：验证角色行为不超出当前知识/能力
4. **章节摘要生成**：每章完成后生成结构化摘要

**集成方式**：
- 方式 A（推荐）：通过脚本/API 调用 DeepSeek
- 方式 B（备用）：生成"审查用文本包"，你手动粘贴到 DeepSeek 界面
