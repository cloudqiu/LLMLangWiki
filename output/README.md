# output/ — 产物输出区

**非 wiki 页面**：这里的文件是过程性产物的落盘记录，不进入 wiki 链接网络、不需要 frontmatter、不列入 `index.md` 的类型分区（仅在 Outputs 区登记一行）。约定见 `CLAUDE.md` §3.2 / §3.3。

放什么：

- **lint 报告**：`YYYY-MM-DD-lint.md`（巡检报告：矛盾 / 过时 / 孤立页 / 数据缺口清单）
- **查询产物**：`YYYY-MM-DD-<slug>.md` 或对应格式（`.png` / `.svg` / `.csv` / Marp 幻灯片等）

不放什么：

- 有沉淀价值的问答与分析 → 回填 `wiki/answers/`（会被 index 收录、被其他页面引用，属于 wiki 层）
- 原始资料 → `raw/`（只读）

登记义务：每次产物落盘后，在 `index.md` 的 Outputs 区加一行，并向 `log.md` 追加 `query` / `lint` 条目。
