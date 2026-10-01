---
title: 英语词汇（Vocabulary）
type: topic
created: 2026-10-01
updated: 2026-10-01
tags: [vocabulary, english, learning]
status: growing
sources: [raw/vocab-inbox.md]
---

# 英语词汇（Vocabulary）

## 概述

本库的英语词汇管理系统（2026-10-01 建立）。分工：

> `raw/vocab-inbox.md` 收词（用户）→ `wiki/vocab/en/` 逐词编译与富化（LLM）→ 词族 / 易混 / 主题综合（持续）→ 复习双轨：SRS 插件排程 + Claude 按需出题。

词条富化含：IPA、词性、双语释义、例句（含翻译）、词根/词族、同义/搭配、易混提示；不确定信息标「待验证」。阅读词条本身即为第一遍复习；`learning` 字段（new → learning → known）跟踪掌握度。

语言分轨（2026-10-01 起）：本页只管英语；法语姊妹系统见 [[french-vocabulary]]（同构、独立收词与计数）。

## 进度

- 词条：**17**（new 17 ｜ learning 0 ｜ known 0）；更新于 2026-10-01
- 待处理：inbox 暂无积压（八批共 17 条已编译，均出自《Mattering》）
- 词条（17）：[[deplete]]、[[off-kilter]]、[[unravel]]、[[turquoise]]、[[flurry]]、[[nook]]、[[counterweight]]、[[disconcert]]、[[hospice]]、[[decor]]、[[unto]]、[[endometriosis]]、[[sew]]、[[competent]]、[[faucet]]、[[wear-on]]、[[in-sight]]（其中 2 条为**多词词条**）
- **待回补清账（2026-10-01 完成）**：[[unto]]、[[endometriosis]]、[[sew]]、[[competent]]、[[faucet]]、[[wear-on]]、[[in-sight]] 共 **7** 条已经 2 批子代理「**WebSearch 转引** etymonline / 医学词典」核对（直连仍 sinkhole）——含实质订正（sew 同源形式按 etymonline 校正、faucet 得名理据改「两说并存」、endometriosis 1927 Sampson 命名核实）；遗留 **3 项**无定论如实保留（in-sight 同源假说、faucet 理据取舍、endometriosis 首见年份两说——见 log 清账条目）。[[counterweight]] 成词年代由第二批子代理复清中；前 10 词编译时已对照核实

## 怎么用

1. **收词**：把生词追加到 `raw/vocab-inbox.md`（一行一个，可附来源/例句）；或在会话中口述「这批词：…」——Claude 先**原样**记入 `raw/vocab-capture-YYYY-MM-DD.md` 再编译。
2. **编译**：说「ingest vocab」→ 去重、逐词建/更新词条（每批 20-50 词）、更新本页、记 log。
3. **复习**：已装 SRS（见下节）——每日 `Ctrl+P → "Review Flashcards from all notes"` 刷卡；想测验就说「考我：最近 20 个 new 词」→ Claude 出题（cloze / 翻译 / 辨析），产物落 `output/`（示例：[[2026-10-01-vocab-quiz-01]]）。

## SRS（复习层，已装）

- 插件：**Spaced Repetition v1.15.4**（st3v3nmw；2026-10-01 安装并启用）
- 当前配置（安装默认值，与本库约定一致）：闪卡标签 `#flashcards` ｜ 单行卡 `::`、双向卡 `:::` ｜ 算法 SM-2-OSR（插件设置里可切 **FSRS**）
- 排程数据：`dataStore = NOTES`——复习后插件的排程信息会以 `<!--SR:…-->` 注释写进词条笔记（正常现象，git 中会见到此类小改动）
- 入口：`Ctrl+P` → "Review Flashcards from all notes"；或侧边栏复习队列（启动时自动打开）

## 结构约定

- 词条路径：`wiki/vocab/en/<word>.md`（小写 lemma；**不逐条进 index.md**）
- **多词词条（2026-10-01 新增约定，因收进 wore on / in sight 而立）**：短语动词与习语同样每词一页，文件名把空格换成连字符（kebab-case），如 `wear-on.md`、`in-sight.md`；frontmatter 的 `word` 保留原样空格（`"wear on"`）；**lemma 取词典 headword，不取遇到形式**——inbox 里的 `wore on` 归并到 `wear on`（同 `unraveling` → [[unravel]] 之例）；pos 用 `phr. v.` / `idiom` 等标注；闪卡正面照写多词 headword。（schema v1.5 已把本条并入 CLAUDE.md §3.4。）
- 标签：`src/<来源>`（如 src/reading、src/书名-slug，标注在哪读到的）、`domain/<领域>`（如 domain/tech、domain/finance，工作/专业领域）——按需取用
- 闪卡：词条「闪卡」小节内**多行双向卡**（`word` ⏎ `??` ⏎ 释义＋词源摘要的背面），小节标题带 `#flashcards/en` 标签——插件凭标签发现卡片、复习时同屏显示词源；法语姊妹系统用 `#flashcards/fr`（见 [[french-vocabulary]]）——牌组树可选 en / fr 分语种刷卡，「all notes」则两语混合；**模板页不写标签**（防 SRS 把占位卡解析成幽灵卡）
- **同名陷阱**：多词词条与单词词条可能只差一个连字符（[[in-sight]] vs `insight`、[[wear-on]] vs `wear`）——建页前先 grep 是否已有近形页面，避免误建重页
- 发现方式：全库按标签自动发现（卡片集中在 `wiki/vocab/en`）

## 词族 / 综合页

（暂无已建页面——出现值得综合的词族、易混对或主题簇时在此登记，并链到对应页面）

- **候选（2026-10-01）**：「直到 / 到」虚词簇 **unto · until · till**——三者同出日耳曼语「直到」义（un- ← 哥特语 und），中古英语中 unto 与 until 一度同义；待 until、till 收词后建一页对照（含 unto / onto 拼写易混）。当前已在 [[unto]] 词条内先行记录
- **候选（2026-10-01）**：**希腊医学构词零件**（endo- / metr- / hyster- / -osis / -itis）——由 [[endometriosis]] 触发的可复用零件库：拆得动 endometriosis，就同时拿下 endometritis、adenomyosis、myometrium、hysterectomy 等一串词（含 -osis 病名 vs -itis 炎症的易混）。词条尚未收词，先登记；愿建页就说一声
- **候选（2026-10-01）**：**同音易混簇 sew · sow · so**（均读 /səʊ/，拼写高发错点）——[[sew]] 已收词，sow / so 未收；再遇同音簇可合并建一页拼写辨析（每词条内已先记录）
- **候选（2026-10-01）**：**wear + 小品词四兄弟**（wear **on** / **off** / **out** / **down**）——[[wear-on]] 已收词，本义动词 `wear` 及另三个短语未收；四条含义相去甚远，值得一页对照（并可挂上「on ＝ 继续下去」这条通用线索：go on / read on / carry on）
- **候选（2026-10-01）**：**sight 家族与介词搭配**（[[in-sight]] · out of sight · on sight · at sight · insight / foresight / hindsight）——核心是「**同源 + 只差一个空格/介词就换义**」：in sight 看得见 vs **insight** 洞察力；in sight vs **on sight**（一见就）。[[in-sight]] 已收词，其余未收

## 开放问题

- 例句来源：当前为 LLM 生成 + 待验证标记；是否引入词典核对流程？
- Dataview（未装）：装上后本页可改为动态统计表（按 learning / tags 查询）。

## 来源

- raw/vocab-inbox.md（收词入口）
- 系统设计：2026-10-01 query 会话（决策记录见 `log.md`）
