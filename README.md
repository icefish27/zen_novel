# 做梦系统 — AI 辅助小说写作

> 凡人修仙传世界观 × 做梦系统 × Claude Code 辅助写作

## 项目结构

```
novel/
├── CLAUDE.md                          # AI 写作规则（自动加载）
├── 小说总纲.md                        # 世界观、系统、故事线、角色（换书时替换此文件）
├── README.md                          # 本文件
├── .claude/commands/                  # 写作命令
│   ├── next-chapter.md                # 一键写一章（写→审→存→更新状态）
│   ├── write-chapter.md               # 写作规则 + 输出格式
│   ├── review-chapter.md              # AI味检测 + 节奏/角色/战力审查
│   ├── update-trackers.md             # 更新章节索引/角色档案/时间线/伏笔
│   └── status.md                      # 查看项目进度
└── output/
    ├── chapters/                      # 正文章节（0001-章名.md）
    ├── drafts/                        # 当前章节草稿
    └── tracking/                      # 随章节更新的"小说状态"
        ├── chapter-index.md           # 每章一句话概要 + 字数
        ├── characters.md              # 角色当前状态（修为/身份/关系）
        ├── timeline.md                # 事件时间线
        └── open-threads.md            # 未收回的伏笔追踪
```

## 怎么用

### 开始写作

在对话中输入：

```
/next-chapter
```

AI 会自动执行：

1. 读 `chapter-index.md` 确认当前写到第几章
2. 读 `characters.md` 获取角色当前状态
3. 读 `open-threads.md` 看有没有要收回的伏笔
4. 读上一章正文，延续文风
5. 读总纲对应段落，确定本章要写什么
6. 按 CLAUDE.md 规则写 2000 字
7. 写完自审（AI味、节奏、角色一致性）
8. 保存正文到 `output/chapters/`
9. 更新所有 tracking 文件

你只需要输入一个命令，剩下的 AI 自己搞定。

### 查看进度

```
/status
```

显示：已写几章、总字数、主角当前修为、未收回伏笔数、总纲路线进度。

### 单独操作

如果你不想跑完整流程：

- `/write-chapter` — 只写，不审不存
- `/review-chapter` — 只审刚写的章节
- `/update-trackers` — 只更新状态文件

### 持续写作

每次想写下一章，输入 `/next-chapter` 就行。AI 会自动接上上一章的结尾和钩子。

## tracking 文件为什么重要

小说写到 50 章、100 章后，不可能把所有章节塞进 AI 上下文。

tracking 文件就是"小说此刻的样子"：

- `chapter-index` 告诉 AI 之前发生了什么（最近 5 章概要）
- `characters` 告诉 AI 每个角色现在什么状态
- `open-threads` 提醒 AI 还有哪些伏笔没收

AI 写新章节时只读 tracking + 上一章正文 + 总纲，不需要翻前面所有章节。省 token，保连贯。

## 写作规则

所有规则在 `CLAUDE.md` 中，AI 每次对话自动加载。核心要点：

- 前 300 字出冲突
- 短句≤17 字，句号断句，禁用"然而/此外/与此同时"
- 避免心理描写太多，可以用生理反应替代
- 短剧五拍节奏：轻踩→加码→反制→引爆→冷收
- 系统提示用电报体
- 苟道流主角：能忍，但踩到线就出手，打完继续装
