# CLAUDE.md — 词汇库 Schema（LLMLangWiki 实例）

> 本目录是 **LLMLangWiki**：一个**词汇专用 LLM Wiki**，2026-10-01 自 LLMWiki 拆分建立，只承载**英语 / 法语词汇系统**（词汇的唯一维护点；原库不再含词汇内容）。
> **本文件是三层架构中的「Schema 层」**：规定目录结构、页面约定与工作流，使 Claude 成为有纪律的词汇库维护者。本文件由用户与 LLM 共同演进；每次修改后在文末「Schema 演进记录」追加一条。

## 0. 角色与总原则

- 维护者：Claude 是本库的**唯一维护者**——词条编译、词源深挖、交叉引用、hub 记账全部由 Claude 完成；用户极少亲手写页面。
- 用户负责：收词（inbox 追加 / 会话口述）、提出查询与测验、复习刷卡、决定方向与重点、在 Obsidian 中阅读与验证。
- 核心原则：**词汇编译一次、持续保鲜**——每词一页、词源与构词一次深挖到位、闪卡排程逐步内化，而不是每次查询重新解释。
- 实践形态：LLM agent 与 Obsidian 分屏——「Obsidian 是 IDE；LLM 是程序员；词汇库是代码库」。

## 1. 目录结构

```
LLMLangWiki/
├── CLAUDE.md             # Schema 层：本文件
├── index.md              # 内容目录：页面清单（每次 ingest 后更新）
├── log.md                # 时间线：append-only（ingest / query / lint / schema / setup）
├── raw/                  # 【原始资料层】只读，永不修改
│   ├── assets/           # 图片等附件（Obsidian 附件目录指向此处）
│   ├── vocab-inbox.md    # 英语收词箱（用户追加；Claude 只读）
│   └── vocab-fr-inbox.md # 法语收词箱（同上）
├── wiki/                 # 【wiki 层】Claude 全权创建与维护
│   ├── vocab/en/         # 英语词条（每词一页，type: vocab；不逐条进 index）
│   ├── vocab/fr/         # 法语词条（同上）
│   ├── topics/           # 主题页：两个 hub + 词族 / 综合页（synthesis）
│   └── answers/          # 从查询中归档的优质问答
├── templates/            # 页面模板（vocab / vocab-fr / topic / answer；建页前先读对应模板）
└── output/               # 【产物区】测验、lint 报告等落盘处；非 wiki 页面
```

## 2. 页面规范

### 2.1 Frontmatter（所有 `wiki/` 下的页面必须有）

```yaml
---
title: 页面标题
type: vocab | topic | answer
created: YYYY-MM-DD
updated: YYYY-MM-DD        # 每次修改页面时更新
tags: []                   # 小写英文标签；词条常用 src/<来源>、domain/<领域>
status: stub | growing | mature | archived
sources: []                # 词条必填：raw/vocab-inbox.md 或 raw/vocab-fr-inbox.md
# 以下按类型补充：
# vocab 页: lang: en | fr, word, ipa, pos, learning: new | learning | known, root（可选）
#           法语词条另加：gender: m. | f. | m. pl. | f. pl.（名词）、plural（名词/形容词复数，不规则必填）
---
```

- `status` 生命周期：`stub`（占位）→ `growing`（持续累积）→ `mature`（稳定）→ `archived`（废弃，须注明原因）。
- `learning`（new → learning → known）跟踪掌握度；复习与测验据此选词。

### 2.2 命名与链接

- 文件名：小写 kebab-case、ASCII、**全库唯一**（Obsidian 按 basename 解析链接，含 `templates/`）。
- 非 ASCII 一律转写（é/è/ê→e、à→a、ç→c、œ→oe、撇号→连字符）；法语细则与命名消歧见 §3.1 第 4 步。
- **多词单位**（短语动词 / 习语 / 多词表达）：同样每词条一页，文件名空格换连字符（`wear-on.md`、`il-y-a.md`）；frontmatter 的 `word` 保留空格原形（`"wear on"`）；lemma 取词典 headword 而非遇到形式。
- 链接统一用 wikilink：`[[decor]]`；需要显示名时 `[[decor|décor]]`。
- 指向尚未创建页面的链接是**允许且有意义**的（待建信号），但必须登记到 `index.md` 的 Backlog 或对应 hub 的待办区。
- 建页前先扫全库（含 `templates/`——`concept.md`、`source.md` 等文件名本身就是法语词）：查近形页（`in-sight` vs `insight`、`wear-on` vs `wear`、`sur` vs `sûr`）与同名页。
- `index.md` / `log.md` / `CLAUDE.md` 用普通文本提及（不建 wikilink）。

### 2.3 语言

- 正文用中文；词条标题即英/法语词本身；术语保留英文（IPA、frontmatter、lemma 等）。
- 词条以 frontmatter `lang: en | fr` 标记语言；两语轨同构、各自独立收词与计数。
- log 条文操作词用英文（ingest / query / lint / schema / setup）；词汇批次用 `vocab:` / `vocab-fr:` 前缀，便于 grep。

## 3. 工作流

### 3.1 Ingest 编译词条（两语轨）

| 语言 | 收件箱（raw/，Claude 只读） | 词条目录 | 模板 | hub | log 标记 | 词源权威源 |
| --- | --- | --- | --- | --- | --- | --- |
| 英语 | `raw/vocab-inbox.md` | `wiki/vocab/en/` | `templates/vocab.md` | [[english-vocabulary]] | `vocab:` | etymonline |
| 法语 | `raw/vocab-fr-inbox.md` | `wiki/vocab/fr/` | `templates/vocab-fr.md` | [[french-vocabulary]] | `vocab-fr:` | CNRTL（TLFi） |

触发词：「ingest vocab」（英语）/「ingest vocab-fr」（法语）。会话口述时先**原样**记入 `raw/vocab-capture-YYYY-MM-DD.md`（英语）或 `raw/vocab-fr-capture-YYYY-MM-DD.md`（法语）再编译，保持可追溯。

1. 读对应 inbox / 捕获文件，与对应词条目录现有词条**去重**（词形变化归并到同一 lemma，如 `wore on` → [[wear-on]]、`suis` → être）；已存在的词只补充不重复建页；lemma 判定存疑时（拼写变体 / 同形多义）在词条内加「收录说明」并在 hub 标注待用户确认。
2. 批量编译（每批 20-50 词）：每词一页（依据对应模板）——IPA、词性、双语释义、例句（含翻译）、词源/构词（词头·词根·词尾拆解、来源与演变、同族/派生；细节对照上表权威源核实）、同义/搭配/易混；不确定信息标「待验证」。
3. 闪卡（每个词条必做）：**多行双向卡**（正面 `word`、分隔行 `??`、背面＝释义＋词源摘要）写在「闪卡」小节；小节标题带语言标签——英语 `#flashcards/en`、法语 `#flashcards/fr`（SRS 子牌组）；复习时卡背同屏显示词源。**单独成行的 `?` / `??` 是插件分隔符**，正文避免。
4. 语言特有要求：
   - **英语**：无额外要求。
   - **法语**：名词必填 `gender` / `plural`；动词在「形态要点」给不定式、组别（1er / 2e / 3e）、助动词、过去分词与现在时六形（**不列表格**）；形容词给阴阳性与复数变化；**每词条必做「英法交叉核对（受影响的英语词）」**——分清**借入**（英 ← 法：路径、年代、拼写·词义漂移）与**共祖**（同源非借入），英语词条已收录则双向互链、无网络标「待验证」。
   - **文件名与消歧（法语细则）**：ASCII 转写（é/è/ê→e、à→a、ç→c、œ→oe、撇号→连字符、保留既有连字符，如 `peut-être` → `peut-etre`）；**全库唯一**——跨语言同名由**后建页者**加语言后缀（先有英语 decor → 法语 `decor-fr.md`），与原词条互链；同语言近形由先建页者保留原形、后建者加词性后缀（`ou-adv.md`），并在法语 hub「命名消歧登记表」登记；避开 **Windows 保留名**（`con` / `aux` / `nul` / `prn` / `com1-9` / `lpt1-9`，加后缀作名）。
5. 出现值得综合的词族 / 易混对 / 主题簇时，建立或更新 `wiki/topics/` 综合页（登记在对应 hub 的「词族 / 综合页」区并互链）。
6. 更新对应 hub（词条计数与待处理清单，更新 `updated`）——**词条不逐条进 index.md**；追加 log：`## [YYYY-MM-DD] ingest | vocab: <批次摘要>`（法语批次用 `vocab-fr:` 前缀，操作词仍为 ingest，便于 grep）。

### 3.2 Query 查询与测验

1. 先读 `index.md` 找候选页面（本库规模内足以替代向量检索）；再读相关 hub 与词条。
2. 带引用作答：wiki 页面用 `[[链接]]`；不确定的事实标「待验证」，不编造。
3. 主要形态：**出题测验**（按 learning / tags / 最近入库选词；cloze / 翻译 / 辨析等题型）、词族与词义辨析讲解、学习计划建议。
4. **产物落盘**：文件型产物（测验、图表、幻灯片等）写入 `output/YYYY-MM-DD-<slug>.<ext>`（约定见 `output/README.md`），对话中引用文件路径；并在 index 的 Outputs 区登记一行、记 log（`query` 条目）。
5. **好答案回填**：有价值的分析、对比、发现的联系，归档为 `wiki/answers/<slug>.md`（依据 `templates/answer.md`），并更新 index（Answers 区）与 log。探索的成果也应当复利。

### 3.3 Lint 巡检

用户要求时执行（建议每月或每积累若干批次后）：

- [ ] 词条间**矛盾**（新旧论断冲突未标注）
- [ ] **过时**内容（已有核对结果推翻，词条未更新）
- [ ] **孤立页**（无任何入链）；hub 的「进度」计数与实际词条数是否一致
- [ ] 「**待回补**」清单是否清理（联网后优先清账：en etymonline / fr CNRTL）
- [ ] **缺失的交叉引用**（同族词、易混对未互链）
- [ ] 闪卡规范（`#flashcards/en|fr` 标签、多行卡格式）是否被破坏

报告落盘为 `output/YYYY-MM-DD-lint.md`，对话中给出摘要；修复动作完成后追加 log：`## [YYYY-MM-DD] lint | <摘要>`。有长期参考价值的结论可另行回填 `wiki/answers/`。

## 4. 导航文件规范

### index.md（内容目录）

- 登记类型分区：Topics（hub / 综合页）/ Answers / Outputs（链接指向 `output/` 文件）/ Backlog。
- **vocab 词条例外**：不逐条入目录（规模考虑）——只登记两个 hub [[english-vocabulary]] 与 [[french-vocabulary]]；词条靠 hub、搜索与 graph 检索。
- 每次 ingest / 产物落盘后必须更新；查询时**先读本文件**。
- **Stats 行格式**：`- vocab 词条：**N**（en **N** ｜ fr **N**） ｜ wiki 页面：**N** ｜ Outputs：**N** ｜ 最近更新：YYYY-MM-DD`。

### log.md（时间线）

- 条目格式：`## [YYYY-MM-DD] <操作> | <标题>`，操作 ∈ {ingest, query, lint, schema, setup}。
- 每条含：动了哪些文件、关键决定、待办。
- 只追加，不修改历史条目（更正以新条目说明）；可用 `grep "^## \[" log.md | tail -5` 查看最近条目。

## 5. 硬性规则（不可违反）

1. `raw/` **永不修改**（不美化、不重排）；inbox 由用户追加，Claude 只读。要批注，写到词条或 wiki 页面里。
2. 每个 wiki 页面必须有 frontmatter；词条 `sources` 指向收词 inbox。
3. 每次 ingest / 产物或答案归档 / lint 报告落盘，必须同步更新 `index.md` 与 `log.md` 及对应 hub。
4. 不静默删页：废弃内容标 `status: archived` 并说明原因。
5. 矛盾必须显式记录，不允许静默覆盖或折中处理。
6. 不确定的事实标注「待验证」，不编造来源；词源一律对照权威源（en: etymonline ｜ fr: CNRTL/TLFi），无网络时标「待验证」并登入对应 hub 的待回补清单。

## 6. 环境与工具

- 本目录是 **Obsidian vault**：附件目录 = `raw/assets/`；模板文件夹 = `templates/`；graph view 用于观察词族 / 孤立页形态。
- **SRS 复习**：Spaced Repetition v1.15.4（`.obsidian/plugins/obsidian-spaced-repetition/`，含 gitignored 的插件本体）。
  - 子牌组：`#flashcards/en`、`#flashcards/fr`（按语言隔离刷卡；"all notes" 两语混合）；**模板页不带标签**（防 SRS 把占位卡解析成幽灵卡）。
  - 算法 FSRS（retention 0.9）；`dataStore = NOTES`——排程以 `<!--SR:…-->` 注释写回词条笔记（正常现象，git 中会见到此类小改动）。
  - 入口：`Ctrl+P` → "Review Flashcards from all notes"。
- 版本管理：**已启用 git**（分支 `main`；仓库级身份）；每次有意义的变更后提交一次。
- 可选工具（未启用，需要时再上）：Dataview（按 frontmatter 出动态统计表）；Marp（幻灯片产物）。

## 7. Schema 演进记录

- **[2026-10-01]** 词汇库 v1.0：自 LLMWiki 拆分建立（fork 自其 Schema v1.6.1）——55 词条（en 17 / fr 38）、两 hub、两模板、两 inbox、测验产物与 SRS 配置整库迁入（逐字节校验）；本文件为词汇专用重写版。继承的词汇 schema 演进（原 LLMWiki §7 摘要）：
  - v1.4（2026-10-01）词汇系统建立：`wiki/vocab/` 每词一页（type: vocab，不逐条进 index）、`templates/vocab.md`、hub、inbox。
  - v1.4.1：「词根 / 词族」升级为「词源 / 构词」——词头·词根·词尾拆解、来源与演变、同族 / 派生、记忆钩；词源细节对照 etymonline 核实。
  - v1.4.2：闪卡升级为**多行双向卡**（`??`），卡背＝释义＋词源摘要，复习同屏可见词源。
  - v1.5：支持**多词单位**（短语动词 / 习语）——kebab-case 文件名、`word` 保留空格原形、lemma 取 headword。
  - v1.6：新增**法语系统**并分语言重构（`vocab/en`、`vocab/fr`）；`lang` / `gender` / `plural` 字段；词源权威源 CNRTL；ASCII 转写与全库唯一命名规则；`#flashcards/en|fr` 子牌组；模板不带标签（防幽灵卡）。
  - v1.6.1：法语词条新增「英法交叉核对（受影响的英语词）」小节（借入 vs 共祖分清）。
- **[2026-10-01]** 迁移记录：拆分建库与逐字节校验见本库 `log.md` 建库条目（原库同日 v1.7 条目对应）。
