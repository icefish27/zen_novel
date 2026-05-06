# Skill: consistency-check

## 用途
利用 DeepSeek V4 Pro 的百万上下文窗口，对全书做一致性审查。这是本系统区别于纯人工/纯单模型的核心能力。

## 适用阶段
Phase 5 — 质量审查（周期性执行）

## 前置输入
- `output/chapters/*.md`（全部已写章节）
- `output/bible/*.md`（世界观设定）
- `output/characters/*.md`（角色档案）
- `output/outline/plot_threads.md`（伏笔表）

## 产出
- `meta/consistency_reports/report_YYYYMMDD.md` — 审查报告

## 执行流程

### 方式 A：API 自动调用（推荐）
- ① 收集所有需要审查的文件内容
- ② 按以下结构组装 Prompt：
  - [系统指令] → 你是一个小说一致性审查专家...
  - [世界观设定] → bible/*.md 的内容
  - [角色档案] → characters/*.md 的内容
  - [伏笔规划] → plot_threads.md 的内容
  - [正文全文] → 所有已写章节
- ③ 调用 DeepSeek API
- ④ 解析返回的审查报告
- ⑤ 写入 meta/consistency_reports/

### 方式 B：手动粘贴（备用）
如果 API 未就绪，系统会生成一个"审查用文本包"，你可以复制到 DeepSeek 界面：
- ① 系统生成 `meta/consistency_reports/_review_package.md`
  - 包含所有上下文 + 审查指令
  - 格式为"复制即用"状态
- ② 你将其粘贴到 DeepSeek
- ③ 将 DeepSeek 的回复粘贴回来
- ④ 我解析并结构化输出

## 审查 Prompt 模板

以下是与 DeepSeek 配合使用的审查指令：

```
【角色】你是一个专业的小说一致性审查专家。
【任务】对以下小说内容进行全面的一致性审查。

【需要你的输出格式】
严格按照以下 JSON 结构输出：

{
  "summary": "整体评估（3-5 句话）",
  "issues": [
    {
      "severity": "critical/major/minor",
      "category": "设定矛盾/角色OOC/伏笔遗漏/时间线错误/力量体系矛盾",
      "description": "问题描述",
      "location": "涉及章节",
      "suggestion": "修复建议"
    }
  ],
  "plot_threads_status": [
    {
      "thread_name": "情节线名称",
      "buried_chapter": "埋设章节",
      "expected_recovery": "预期回收章节",
      "current_status": "已埋设/发展中/已回收/遗漏",
      "note": "备注"
    }
  ],
  "character_consistency": [
    {
      "name": "角色名",
      "issues_found": ["问题1", "问题2"],
      "overall_assessment": "总体评价"
    }
  ],
  "strengths": ["做得好的地方1", "做得好的地方2"],
  "action_items": ["首要修复项1", "首要修复项2"]
}

【审查内容开始】

=== 世界观设定 ===
{world_bible_content}

=== 角色档案 ===
{character_files_content}

=== 伏笔规划 ===
{plot_threads_content}

=== 正文全文 ===
{all_chapters_content}

【审查内容结束】

请开始审查。
```

## 审查周期建议

| 字数规模 | 审查频率 | 上下文范围 |
|---------|---------|-----------|
| 0-5 万字（~20章） | 每写完10章 | 全书 |
| 5-20 万字（~80章） | 每15章 | 全书 |
| 20-50 万字（~200章） | 每20章 | 全书（尚在 1M 窗口内） |
| 50万字+ | 每卷完成后 | 本卷 + 前卷摘要 |

## 常见审查发现项

| 类别 | 示例 |
|------|------|
| 设定矛盾 | "第3章说主角是孤儿，第15章出现了他的父亲" |
| 角色 OOC | "第8章主角的性格果断狠辣，第12章突然优柔寡断" |
| 伏笔遗漏 | "伏笔'神秘裂缝'于第5章埋设，200章未回收" |
| 力量体系 | "主角筑基期打败了金丹期强者，但缺乏合理解释" |
| 时间线 | "第10章说'三天后'，但事件数量明显超过三天" |
| 名字错误 | "配角的名字在前100章和后100章不一致" |
| 擦边红线 ⚠️ | 暧昧互动是否踩到番茄审核红线？是否有不适合平台的内容？ |
| 擦边角色一致 | 女主/女配对主角的态度是否前后一致（不能突然变冷淡或变主动）？ |
| 写作风格违规 | 是否有散文式堆砌词藻的段落？是否有可以压缩的水文？ |
