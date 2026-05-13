执行完整的新章节写作流程：

1. 读 `output/tracking/chapter-index.md` 确认当前进度，确定下一章是第几章、属于总纲哪一段。
2. 读 `output/tracking/open-threads.md` 看有没有能顺手收回的伏笔。
3. 读 `output/tracking/characters.md` 获取本章出场角色的当前状态。
4. 读上一章正文（`output/chapters/` 下最新文件）延续文风。
5. 读 `小说总纲.md` 中本章对应的段落。
6. 读 `.claude/commands/write-chapter.md`，按其中的写作规则和输出格式写 2000 字正文。
7. 正文写完后，读 `.claude/commands/review-chapter.md`，按检查清单逐项审稿，发现问题当场改。
8. 审稿通过后，正文保存到 `output/chapters/XXXX-章名.md`。
9. 读 `.claude/commands/update-trackers.md`，按更新操作更新所有 tracking 文件。
