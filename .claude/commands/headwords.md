---
description: 刷新词头总表（英语 + 法语）→ output/YYYY-MM-DD-vocab-headwords.md
argument-hint: "[可选：en / fr —— 只出单语清单]"
---

刷新本库的**词头总表**产物（LLMLangWiki 词汇库；schema 见 `CLAUDE.md`）。

参数（可选）：`$ARGUMENTS` —— 给 `en` 或 `fr` 时只出该语言的清单，留空则英法两份都出。

## 步骤

### 1. 提取（只读）

对 `wiki/vocab/en/` 与 `wiki/vocab/fr/` 下每个 `.md`，从 **frontmatter** 取 `word`、`learning` 字段，连同文件名 basename：

```powershell
Get-ChildItem wiki/vocab/en, wiki/vocab/fr -Filter *.md | ForEach-Object {
  $t = Get-Content $_.FullName -Raw
  $w = [regex]::Match($t, '(?m)^word:\s*(.+)$').Groups[1].Value.Trim().Trim('"')
  $l = [regex]::Match($t, '(?m)^learning:\s*(.+)$').Groups[1].Value.Trim()
  [pscustomobject]@{ lang = $_.Directory.Name; word = $w; learning = $l; file = $_.BaseName }
}
```

同时取出「`word` ≠ 文件名」的条目（重音转写 / 多词单位 / 词性后缀与保留名三类）。

### 2. 核对（重要）

把 `en` / `fr` / 合计三个计数与两处记账比对：

- `wiki/topics/english-vocabulary.md`、`wiki/topics/french-vocabulary.md` 的「词条：**N**」行
- `index.md` 的 Stats 行

**不一致就先把差异报告给用户**（哪边多了少了、差几个），**不要静默改数**；按 `CLAUDE.md` §5 第 5 条，矛盾必须显式记录。

### 3. 落盘

写 `output/YYYY-MM-DD-vocab-headwords.md`（当天日期；同名已存在则覆盖——该文件是**快照**，不是历史累积）。版式沿用既有产物，顺序为：

1. 统计（en ｜ fr ｜ 合计；`learning` 分布）
2. 英语按字母（标出多词 / 含连字符条目）
3. 法语按字母
4. **附：文件名 ≠ 词形**对照表三小节 —— ①重音 / 特殊字符转写 ②多词单位（空格 → 连字符）③词性后缀 / 保留名（消歧）
5. 生成脚本
6. 生成日期与计数一致性说明

**务必保留两处特例说明**：`la` 双页（[[la-art]] 冠词 / [[la-pron]] 宾语代词）；`en` 指 [[en-pron]]。

### 4. 记账

- `index.md`：Outputs 区更新该产物那一行（无则新增一行）；Stats 行的 `Outputs：**N**` 计数与「最近更新」日期
- `log.md`：追加 `## [YYYY-MM-DD] query | 词头总表：en N + fr N = N`，内容含：产物路径、与 hub 计数是否一致、本次是否有新词条并入

### 5. 回报（对话里）

两个数各是几、与 hub 是否一致、产物路径、以及**如有不符则列出差异**。

## 纪律

- `raw/` **永不修改**（收词箱只读）
- 词条**不逐条进** `index.md`（词条靠 hub 与搜索）——本命令只产出 `output/` 快照
- 不擅自改 hub 计数：发现差异先报告，经用户确认再改
- 产物属 `output/` 层：**不需要 frontmatter**，只在 index 的 Outputs 区登记一行
