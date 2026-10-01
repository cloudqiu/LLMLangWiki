---
title: 法语词汇（Vocabulaire）
type: topic
created: 2026-10-01
updated: 2026-10-01
tags: [vocabulary, french, learning]
status: growing
sources: [raw/vocab-fr-inbox.md]
---

# 法语词汇（Vocabulaire）

## 概述

本库的**法语**词汇管理系统（2026-10-01 建立），与英语系统 [[english-vocabulary]] 同构分轨、独立收词与计数：

> `raw/vocab-fr-inbox.md` 收词（用户）→ `wiki/vocab/fr/` 逐词编译与富化（LLM）→ 词族 / 易混 / 主题综合（持续）→ 复习双轨：SRS 子牌组 `#flashcards/fr` + Claude 按需出题。

词条富化含：IPA、词性、**阴阳性与复数**、**形态要点**（动词：不定式·组别·助动词·过去分词·现在时六形，不列表格）、双语释义、例句（含翻译）、词源/构词（对照 **CNRTL（TLFi）** 核实）、**英法交叉核对**（受影响的英语词：借入 vs 共祖）、同义/搭配/易混（含 faux amis）；不确定信息标「待验证」；`learning` 字段（new → learning → known）跟踪掌握度。

## 进度

- 词条：**38**（new 38 ｜ learning 0 ｜ known 0）；更新于 2026-10-01
- 待处理：inbox 暂无积压（五批共 38 条已编译；前 5 条标注 Duolingo，第三至五批 inbox **未标注来源**——推断 Duolingo，**待确认**）
- 词条（38）：[[gouvernement]]、[[cathedrale]]、[[patissier]]、[[chapitre]]、[[detective]]、[[il-y-a]]、[[environ]]、[[mot]]、[[nouveau]]、[[auteur]]、[[connaitre]]、[[court]]、[[presentateur]]、[[certain]]、[[trouver]]、[[emission]]、[[decouvrir]]、[[incroyable]]、[[deja]]、[[voir]]、[[litterature]]、[[moderne]]、[[prendre]]、[[ca]]、[[mon]]、[[frere]]、[[chercher]]、[[son]]、[[deodorant]]、[[pouvoir]]、[[jamais]]、[[rester]]、[[longtemps]]、[[souffle]]、[[bougie]]、[[gateau]]、[[sur-prep]]、[[inacceptable]]
- **待回补（联网后）**：共 **38** 条未对照 CNRTL（年代与借入路径细节逐条标「待验证」）。法语词源一律以 CNRTL/TLFi 为准

## 怎么用

1. **收词**：追加到 `raw/vocab-fr-inbox.md`（一行一个，可附来源/例句；**重音、撇号照写**）；或会话口述——Claude 先**原样**记入 `raw/vocab-fr-capture-YYYY-MM-DD.md` 再编译。
2. **编译**：说「**ingest vocab-fr**」→ 去重、逐词建/更新词条（每批 20-50 词）、更新本页、记 log（`ingest | vocab-fr: …`）。
3. **复习**：SRS 刷卡（法语卡在 `flashcards/fr` 子牌组，可只复习法语）；想测验就说「考我：最近 20 个法语 new 词」→ Claude 出题，产物落 `output/`。

## SRS（复习层，与英语共用插件）

- 插件：Spaced Repetition v1.15.4（同一实例、同一配置：闪卡标签 `#flashcards`，**子标签自动纳入**）
- 法语卡：小节标题 `## 闪卡 #flashcards/fr` → 归入 **flashcards/fr 子牌组**（英语为 `flashcards/en`；复习「全部」时两语混合，牌组树里选 fr 可隔离）
- 排程数据：`dataStore = NOTES`——排程信息以 `<!--SR:…-->` 注释写进词条笔记
- 入口：`Ctrl+P` → "Review Flashcards from all notes"

## 结构约定

- 词条路径：`wiki/vocab/fr/<word>.md`（小写 lemma；**不逐条进 index.md**）
- **lemma 规则**：动词→不定式（遇到 `suis` 归并到 `être`）；名词→单数、不带冠词（阴阳性进 `gender` 字段）；形容词→阳性单数；代词式动词带 se（`se laver` → `se-laver.md`）；多词表达取词典 headword
- **文件名一律 ASCII 转写**（重音只留在 `word` 与正文/卡片里）：

  | 法语字符 | 转写 | 例 |
  | --- | --- | --- |
  | é è ê ë | e | école → `ecole.md` |
  | à â | a | là → `la.md`（与 `la` 撞名 → 消歧） |
  | î ï | i | île → `ile.md` |
  | ô | o | tôt → `tot.md` |
  | ù û | u | où → `ou.md`（与 `ou` 撞名 → 消歧） |
  | ç | c | français → `francais.md` |
  | œ æ | oe / ae | cœur → `coeur.md` |
  | 撇号 ' ’ | - | l'air → `l-air.md`；aujourd'hui → `aujourd-hui.md` |
  | 连字符 | 保留 | peut-être → `peut-etre.md` |
  | 空格（多词单位） | - | avoir l'air → `avoir-l-air.md` |

  理由：Obsidian 按 basename 解析链接，而 é / œ 存在 NFC/NFD 两种编码——ASCII 化杜绝重影页面。

- **命名消歧**（basename 必须**全库唯一**；建页前先扫全库，含 `templates/`——`concept.md`、`source.md` 等本身就是法语词）：
  - 跨语言同名（décor / decor、hospice、turquoise、compétent / competent；反向例 [[detective]]）：**后建页者**加语言后缀——法语后建用 `-fr`（`decor-fr.md`）、英语后建用 `-en`（`detective-en.md`），两页互链
  - 同语言近形（ou / où、a / à、la / là、sur / sûr、du / dû、cote / côte / coté）：**一律先用带词性后缀的名字**（`ou-conj.md` / `ou-adv.md`），避免「先到先得」的任意性，并登记下表
  - **Windows 保留名**（con / prn / aux / nul / com1-9 / lpt1-9）加后缀（`con-adj.md`、`aux-prep.md`）
- 标签：`src/<来源>`、`domain/<领域>`；语言由 frontmatter `lang: fr` 标记
- **英法交叉核对**（v1.6.1，每词条必做）：查出受该法语词影响的英语词——**借入**（英 ← 法：路径、年代、拼写/读音漂移、词义异同）与**共祖**（同源非借入）分清，反向回借（法 ← 英）可留意；英语词库已收录则双向互链（参见 décor ↔ [[decor]]）；无网络标「待验证」
- 闪卡：`## 闪卡 #flashcards/fr` 内**多行双向卡**；正面＝词（照写重音），背面＝释义＋形态＋词源摘要；阴阳性 / 变位可加挖空卡（`==…==`）。**单独成行的 `?` / `??` 是插件分隔符**，正文避免
- 发现方式：全库按标签自动发现（卡片集中在 `wiki/vocab/fr`）

## 命名消歧登记表

| 页面 | 原形 | 说明 |
| --- | --- | --- |
| [[detective]]（法语） | detective | 英语 detective 若日后收词建页，按「后建者加语言后缀」规则记为 `detective-en.md`，两页互链 |
| [[sur-prep]]（法语） | sur | sur（介词）与 sûr（确定的，adj.）近形对——预置词性后缀 `sur-prep.md`；日后收 sûr 记为 `sur-adj.md` |

## 词族 / 综合页

（暂无已建页面——出现 faux amis 簇、同音簇、构词零件（-tion / -ment / re- / dé-）或英法同源对（décor ↔ [[decor]]）等值得综合的内容时在此登记）

- **候选（2026-10-01）**：「掌舵」词族 **gouvernement · gouverner · gouverneur · gouvernail · gouvernante · gouvernance**——同出拉丁 gubernare ← 希腊 kybernan「掌舵」，与英语 govern / governor / cybernetics / cyber- 同源；由 [[gouvernement]] 触发，待再收两三词后评估建综合页
- **候选（2026-10-01）**：**caput「头」词族**（[[chapitre]] · 英语 chief / chef / capital / captain / cape）——chief 与 chef 是英语对同一词源的双重借入；待再收 capitaine / capital 类法语词后评估建页
- **候选（2026-10-01）**：**cathedra「座」词族**（[[cathedrale]] · 英语 chair / chaise / cathedra）——chair 与 cathedral 同出拉丁 cathedra（双重借入），chairman 的 chair 也在这一线
- **候选（2026-10-01）**：**pasta「面团」词族**（[[patissier]] · pâte · 英语 pastry / paste / pasta / pâté）——paste 与 pasta 双重借入、pâté 再借入；「一个词根多次进英语」的样本
- **候选（2026-10-01）**：**英语回借簇（anglicismes）**（[[detective]] 起）——détective 之后若再收 week-end / email / marketing 等，可合并一页「法语里的英语借词」
- **候选（2026-10-01）**：**connaître vs savoir**（认识 vs 知道）——法语动词辨析的头号易混对：connaître 认识（+ 名词，凭经验）／ savoir 知道（+ 从句 / 不定式）；由 [[connaitre]] 触发，待 savoir 收词后建辨析页
- **候选（2026-10-01）**：**同音簇 court · cour · cours**（都读 /kuʁ/，另有 il court「他跑」）——短 / 院子·宫廷 / 课程，一个音四个义；由 [[court]] 触发
- **候选（2026-10-01）**：**「新」三兄弟 nouveau · neuf · récent**＋英语 new / novel 的**借入 vs 共祖**样本——由 [[nouveau]] 触发（new 是 PIE 共祖、novel 是古法语借入，最佳对比例）
- **候选（2026-10-01）**：**prehendere「抓」词族**（[[prendre]] · apprendre · comprendre · surprendre · entreprendre ＋ 英 entrepreneur / enterprise / surprise / prison）——surprise 的 -prise 即「抓」；由 [[prendre]] 触发，待收 comprendre / apprendre 后建页
- **候选（2026-10-01）**：**videre「看/知」词族**（[[voir]] · revoir · prévoir ＋ 英 view / video / vision / wit / wise）——PIE \*weid- 的「看」与「知」一体两面；由 [[voir]] 触发
- **候选（2026-10-01）**：**credere「信」词族**（croire · [[incroyable]] · croyance ＋ 英 credit / creed / credible / incredible 平行构词）——由 [[incroyable]] 触发
- **候选（2026-10-01）**：**capere「拿」词族**（[[inacceptable]] · accepter · recevoir · concevoir ＋ 英 accept / receive / capture / perception）——「拿」串起接受/接收/构想；由 [[inacceptable]] 触发
- **候选（2026-10-01）**：**主有词 mon · ton · son 系列**（[[mon]] · [[son]] · ma/ta/sa · mes/tes/ses）——性数配合 + 元音前阴名用 mon/son/ton 的规则；由 [[mon]] / [[son]] 触发
- **候选（2026-10-01）**：**stare「站」词族**（[[rester]] · arrêter · être ＋ 英 rest / arrest / stand / stay / state）——rester 的「停留」＝「站住不走」；由 [[rester]] 触发

## 开放问题

- 例句来源：当前为 LLM 生成 + 待验证标记；是否引入 CNRTL / Le Robert 核对流程？
- 「形态要点」是否提升为 frontmatter 字段（便于日后 Dataview 查询）？
- 英语系统的 etymonline 待回补（7 条）与本系统 CNRTL 待回补，联网后一并清账。

## 来源

- raw/vocab-fr-inbox.md（收词入口）
- 系统设计：2026-10-01 会话，schema v1.6（决策记录见 `log.md`）
