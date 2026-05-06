# 禅机 — AI 辅助小说创作系统

> Claude Code + DeepSeek V4 Pro 驱动的长篇小说创作工作流。

## 核心理念

这不是一个"一键写书"的工具，而是一套**可执行的创作工作流**。

Claude Code 作为调度中枢，调用 7 个 Skill 覆盖从世界观构建到正文写作的全流程；DeepSeek V4 Pro 的百万上下文窗口负责全书级的一致性审查。两个模型互补，你在终端中与系统对话即可完成整本小说的创作。

## 快速开始

```bash
# 启动新书项目
/new-book

# 查看当前进度
/status

# 写下一章
/next-chapter
```

## 系统架构

```
system/              → 架构设计（Agent 定义 / Skill 总览 / 工作流）
skills/              → 7 个 Skill 定义文件
templates/           → 可复用的写作模板
output/              → 小说产出目录
  ├── bible/         世界观
  ├── characters/    角色档案
  ├── outline/       大纲体系
  ├── chapters/      正文章节
  ├── meta/          元数据（自动维护）
  └── analysis/      分析报告
checklist.md         → 新书启动问卷（40 题）
```

## 工作流

| 阶段 | 内容 | 产出 |
|------|------|------|
| Phase 0 | 填写 checklist 启动项目 | novel.config.yaml |
| Phase 1 | 世界观构建 | bible/*.md |
| Phase 2 | 角色设计 | characters/*.md |
| Phase 3 | 大纲规划 | outline/*.md |
| Phase 4 | 正文写作 | chapters/*.md |
| Phase 5 | 一致性审查 | meta/consistency_reports/ |
| Phase 6 | 发布准备 | 平台适配 |

## 技术栈

- **Claude Code** — 调度中枢 + 创作 Agent
- **DeepSeek V4 Pro** — 长上下文一致性审查
- **Markdown + YAML** — 文件系统即数据库

## 许可证

MIT
