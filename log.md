# Log — 时间线（LLMLangWiki）

> 追加式（append-only）记录：ingest / query / lint / schema / setup。
> 条目格式 `## [YYYY-MM-DD] <操作> | <标题>`；`grep "^## \[" log.md | tail -5` 可看最近 5 条。
> 只追加，不修改历史条目（更正以新条目说明）。

> **继承说明（2026-10-01）**：本库自 LLMWiki 拆分建立；以下 22 条词汇相关记录逐字继承自原库 `log.md`（原库未改动）。原库同期其他条目未收录；继承条目中指向原库页面的个别链接（如 personal-knowledge-management）不在本库，保留原文、不解析。

## [2026-10-01] query | 词汇管理方案设计（英语生词）

- 问题：用户希望用本库管理已收集的英语词汇（散落各处未整理；以「阅读遇到的词」与「工作/专业词汇」为主）
- 方案要点：`raw/vocab-inbox.md` 收词（用户）→ `wiki/vocab/<word>` 逐词编译与富化（LLM，含闪卡行）→ 词族/易混/主题综合 → 复习双轨（SRS 插件排程 + Claude 按需出题）
- 决策（用户选定）：① 复习 = SRS 插件 + Claude 出题；② 无从批量导入，自 inbox 逐步收录；③ 标签以来源（src/）+ 领域（domain/）为主
- 回填：要点落 [[english-vocabulary]]（hub）；schema 变更见下一条
- 插件调研（2026 现状）：Spaced Repetition（经典生态）、LearnKit / EngramQuest（FSRS）、English Vocabulary Learning Plugin / Klomimory（语言学习专用）、Glossa / AI Flashcard Generator（AI 制卡）——选型待定，记于 hub

## [2026-10-01] schema | 词汇系统 v1.4

- 新增：`wiki/vocab/`（新页面类型 vocab，每词一页、不逐条进 index）、`templates/vocab.md`、hub 页 [[english-vocabulary]]、收件箱 `raw/vocab-inbox.md`（用户追加；Claude 只读）
- 修改 `CLAUDE.md`（v1.4）：§1（结构 +vocab/）、§2.1（vocab 页 frontmatter 字段）、§3.4（新增「Vocab 词汇编译」工作流）、§4（vocab 例外 + Stats 行格式）、§7（v1.4 记录）
- 更新：`index.md`（Topics + Stats + Backlog）；[[personal-knowledge-management]]（记忆方法开放问题 → 已启动 + 页面地图）
- 待办：SRS 插件选型与安装；首批词条待用户收词后编译

## [2026-10-01] ingest | vocab: 首批 3 词（deplete / off-kilter / unravel）

- 来源：`raw/vocab-inbox.md`（用户追加）；三词均出自书籍《Mattering》→ 标签 `src/mattering`
- 新建词条 3 个：[[deplete]]、[[off-kilter]]、[[unravel]]（遇到形式 "unraveling" 归并至 lemma）
- 首次执行 §3.4 词汇编译流程；更新 [[english-vocabulary]]（进度 0→3 + 最近入库）、`index.md`（Stats）
- 备注：例句为 LLM 生成；词源细节（kilter 词源不明、ravel 荷兰语来源）已在词条内说明
- 待办：SRS 插件仍未装（见 hub 开放问题）；随积累考虑词族 / 易混综合页

## [2026-10-01] query | vocab 测验 #1（deplete / off-kilter / unravel）

- 首次复习测验：Claude 出题 9 道（填空 3 / 英译中 2 / 中译英 2 / 短语 1 / 辨析 1），对话内作答
- 产物落盘：`output/2026-10-01-vocab-quiz-01.md`（含答案解析与作答记录区）；index.md Outputs 区首条 + Stats（Outputs 0→1）
- 评分结果随后记入产物文件（不改历史条目）

## [2026-10-01] setup | SRS 插件安装（Spaced Repetition v1.15.4）

- 用户完成安装并启用：`.obsidian/plugins/obsidian-spaced-repetition/`；`.obsidian/community-plugins.json` 启用清单已入库
- 配置核对（安装默认值，与词条约定一致）：flashcardTags=`#flashcards`、分隔符 `::` / `:::`；算法 SM-2-OSR（FSRS 可选未切）；dataStore=NOTES（排程将以 `<!--SR:…-->` 注释写回词条笔记）
- `.gitignore` 追加：插件本体（main.js / styles.css）不入库，仅跟踪启用清单与插件 manifest/data（设置）
- hub [[english-vocabulary]]：新增「SRS（复习层，已装）」小节，移除开放问题中的选型项；index Backlog 移除该项
- 待办：首批卡已就绪（3 词条）——可开始首次复习

## [2026-10-01] query | 词源深挖：deplete / off-kilter / unravel（etymonline 核对）

- 请求：为现有词条补全词源深挖（词头 / 词根 / 词尾要素、来源与演变）
- 词源要点：**deplete**＝1807 年由 depletion 反向构词（拉丁 dēplēre「倒空」，PIE \*pele-「填满」的家族里唯一的「倒空」）；**unravel**＝un-²（反向前缀）+ 荷兰语 ravelen，c.1600，附带「ravel 同义又反义」悖论；**off-kilter**＝kilter 词源不明（方言 kelter c.1600），out of kilter 首见 1620s，off- 为「偏离常规」组合成分
- 落地：3 个词条「词根/词族」→「词源/构词」升级（含拆解/演变/同族/记忆钩）；`templates/vocab.md` 与 CLAUDE.md §3.4 同步（v1.4.1）
- 核对依据：etymonline（deplete / unravel / ravel / kilter 四页）；日期与词形均采用其记载，不作推测
- 备注：data.json 显示算法仍为 SM-2-OSR（FSRS 切换待用户在插件设置中完成）

## [2026-10-01] query | 闪卡显示词源：卡格式升级为多行双向卡（??）

- 需求：复习卡片时同屏显示词源深挖内容
- 机制核对（官方文档 stephenmwangi.com）：多行卡 `?` / 双向 `??`，两侧可多行、须「贴合」分隔行；卡片只渲染卡内文本，不自动带出页面其它段落——故将词源摘要写入卡背
- 变更：3 个词条闪卡改为多行双向卡（背面＝释义＋词源摘要＋同族/联想）；templates/vocab.md、CLAUDE.md §3.4 与 §7（v1.4.2）、hub 结构约定同步
- 备注：旧卡无复习历史（scheduleData 为空），重构无损失；如需主动测验词源可另加挖空卡（`==…==`），暂不默认

## [2026-10-01] ingest | vocab: 第二批 3 词（turquoise / flurry / nook）

- 来源：`raw/vocab-inbox.md`；均出自《Mattering》→ `src/mattering`；去重跳过已编译的 3 词（deplete / off-kilter / unravel）
- 新建词条 3 个：[[turquoise]]、[[flurry]]、[[nook]]——按 v1.4.1 标准（词源对照 etymonline）+ v1.4.2 卡格式（多行双向卡，卡背含词源）
- 词源要点：**turquoise**＝法语「土耳其石」（取道土耳其传入欧洲，实产波斯）；**flurry**＝1690s「一阵疾风」，来源不确定（疑拟声）；**nook**＝c.1300「角落」，来源不明（或古诺斯语 nokke）
- 更新：[[english-vocabulary]]（进度 3→6，词条列表全列）、`index.md`（Stats）
- 待办：无积压；SRS 已装、可开始首刷（词条已达 6 个，够刷一轮）

## [2026-10-01] ingest | vocab: 第三批 2 词（counterweight / disconcert）

- 来源：`raw/vocab-inbox.md`（《Mattering》）→ src/mattering；去重跳过 6 词
- 新建词条 2 个：[[counterweight]]、[[disconcert]]（遇到形式 disconcerting 归并至 lemma）
- 词源要点：**counterweight**＝透明复合词（counter- ← 拉丁 contra；weight ← 古英语 / PIE \*wegh-）；**disconcert**＝1680s，法语 disconcerter ← 意大利 concertare「使协调」，dis- 反转之；同根 \*krei- 家族（certain / discern / crisis）
- 备注：etymonline 与 Wiktionary 均无 counterweight 成词年代——已按 schema 标「待验证」而非编造
- 更新：[[english-vocabulary]]（6→8）、`index.md`（Stats）

## [2026-10-01] ingest | vocab: 第四批 2 词（hospice / decor）

- 来源：`raw/vocab-inbox.md`（《Mattering》）→ src/mattering；去重跳过 8 词
- 新建词条 2 个：[[hospice]]、[[decor]]
- 词源要点：**hospice**＝1818「旅人招待所」→1879「临终机构」；拉丁 hospes（客人/主人），与 hospital / hotel / hostile 同出 PIE \*ghos-ti-「陌生人」家族；**decor**＝1897（原为剧场用语），法语 décor 逆构自 décorer ← 拉丁 decorare，英语恰好还原了拉丁名词 decor 本身；同根 \*dek-（decent / dignity / doctor / orthodox）
- 更新：[[english-vocabulary]]（8→10）、`index.md`（Stats）
- 备注：hospice 词条未采纳「运动始于 1960s 英国」作例句（无本批来源支撑），改用中性例句

## [2026-10-01] ingest | vocab: 第五批 1 词（unto）

- 来源：`raw/vocab-inbox.md`（《Mattering》）→ src/mattering；去重跳过 10 词（`unraveling` 归并至 lemma [[unravel]]）
- 新建词条 1 个：[[unto]]（本批 inbox 仅新增 1 词，git diff 已确认）
- 词源要点：**unto**＝「直到、及于」义的日耳曼语前缀（哥特语 und / 古诺斯语 unz / 古高地德语 unz）＋ 古英语 tō「到」，约 1300 年入英语；与 until / till 同族（中古英语中 unto 与 until 一度同义），早期现代英语兼作 to 的「与格」形式（KJV：spake unto Moses），19 世纪后退入文学/法律/宗教语体
- 易混提示：unto / to / until / onto（unto↔onto 仅差一字母）
- **备注（环境限制）**：本会话出网被整体 sinkhole（所有域名解析至保留段 198.18.0.0/15，`web_fetch` 全线失败），etymonline / Wiktionary **无法访问**——词源按既有知识撰写，「成词机制（un-+to 复合 vs. until 类推）」与「印欧语更早来源」已标**待验证**；恢复联网后应回补 etymonline 核对（词条与 hub 均已记该待办）
- 更新：[[english-vocabulary]]（进度 10→11、新增待回补项、词族候选 unto/until/till）、`index.md`（Stats：vocab 10→11）

## [2026-10-01] ingest | vocab: 第六批 1 词（endometriosis）

- 来源：`raw/vocab-inbox.md`（《Mattering》）→ src/mattering；去重跳过 11 词
- 新建词条 1 个：[[endometriosis]]（首个非通用语域词条；新增 `domain/medical` 标签，为该词条判断——若与 hub 的「工作/专业领域」约定不符可撤）
- 词源要点：**endometriosis**＝endometrium（19 世纪解剖学术语：endo- 希腊「在内」+ metr- 希腊 mētra「子宫」+ -ium）再挂希腊病理后缀 -osis；字面「内膜的病变状态」；一般追溯到 John A. Sampson（1927）命名——**待验证**（无网络）
- 易混要点：endometritis（-itis 炎症，一字之差）、adenomyosis（内膜进肌层）、endometrium（正常内膜）；同族 metr- 与 hyster- 两套「子宫」词根
- **备注（环境限制）**：出网仍被 sinkhole（198.18.0.0/15），etymonline / 医学词典不可达；「1927 年 Sampson 造词」标待验证，与 [[unto]] 一并留待联网回补
- **顺带发现的仓库杂物**：根目录出现空文件 `other-word.md`（2026-10-01 12:47 生成）——成因是 `templates/vocab.md` 里作占位示例的 wikilink `[[other-word]]` 被点击后由 Obsidian 自动建note。未删除（schema 规则 4：不静默删页），待用户定夺；建议把模板与 hub 中的该占位改为反引号 `other-word` 以免再生
- 更新：[[english-vocabulary]]（进度 11→12、待回补项并入本词、新增「希腊医学构词零件」词族候选）、`index.md`（Stats：vocab 11→12）

## [2026-10-01] ingest | vocab: 第七批 3 词（sew / competent / faucet）

- 来源：`raw/vocab-inbox.md`（《Mattering》）→ src/mattering；去重跳过 12 词
- 新建词条 3 个：[[sew]]、[[competent]]、[[faucet]]
- 词源要点：**sew**＝古英语 siwian ← 原始日耳曼语 \*siwjanan ← PIE \*syu-「缝、绑」，同族 seam / suture / couture，本族词故拼写与变化不规则；**competent**＝拉丁 com-「共同」+ petere「追求」→ competere「共同争取、相合」→「够得上标准」，与 compete/competition 同根，同族 petition / appetite / impetus / repeat（PIE \*pet-），另有法律义「有管辖权的、有行为能力的」（competent authority）；**faucet**＝古法语 fausset「塞子、龙头」← fausser「弄坏」← 拉丁 falsus「假的」（false/fail/fault 家族），约 1400 年入英语，原指酒桶取酒塞嘴
- 易混要点：**sew / sow / so 三词同音 /səʊ/**（拼写高发错点，已登记词族候选）；competent vs qualified / capable / proficient（并记「merely competent」的贬义语用）；**faucet（美） vs tap（英）**，拼读 au=/ɔː/、-et=/ɪt/
- **备注（环境限制）**：出网仍被 sinkhole（先 `Resolve-DnsName` 复测确认解析仍指向 198.18.0.0/15，`Invoke-WebRequest` 与 `web_fetch` 均失败），etymonline 不可达——三词的年代/理据细节逐条标「待验证」
- **核对债务**：待回补清单累计 5 词（unto / endometriosis / sew / competent / faucet），已在 hub 显式标记「债务在累积」，待恢复出网后一次性清账
- 更新：[[english-vocabulary]]（进度 12→15、待回补清单列全、新增同音簇词族候选）、`index.md`（Stats：vocab 12→15）
- **更正上一条（第六批）**：占位 wikilink `[[other-word]]` **只存在于 `templates/vocab.md:30`**，hub 页并无该占位（已 grep 全库确认）——上条「模板与 hub 中的该占位」表述有误，修正为仅模板；`other-word.md` 已从磁盘消失（用户清理），无需再处理

## [2026-10-01] schema | v1.5 词汇系统支持多词单位（短语动词 / 习语）

- 触发：本批 inbox 收进 `wore on`、`in sight`——**首次出现多词单位**，而 §3.4 原文只写「每词一页 `wiki/vocab/<word>.md`」，无命名与 lemma 依据；为免 wiki 层自行发明约定而无 schema 支撑，当批一并立规
- 新规：多词单位同样每词条一页——文件名空格换连字符 kebab-case（`wear-on.md`、`in-sight.md`）；frontmatter `word` 保留空格原形（`"wear on"`）；**lemma 取词典 headword，不取遇到形式**（`wore on` → `wear on`，同 `unraveling` → [[unravel]] 之例）；pos 标 `phr. v.` / `idiom`；建页前先查近形页（`in-sight` vs `insight`、`wear-on` vs `wear`）
- 同步：`CLAUDE.md` §3.4 第 2 步 + §7 追加 v1.5；hub「结构约定」加多词条款与「同名陷阱」提示
- 备注：本变更**源自用户收词行为本身，非用户显式请求**——已在 §7 注明「如不认可可回退」，等用户表态

## [2026-10-01] ingest | vocab: 第八批 2 条（wear on / in sight）——首批多词词条

- 来源：`raw/vocab-inbox.md`（《Mattering》）→ src/mattering；去重跳过 15 条
- 新建词条 2 条：[[wear-on]]（遇到形式 `wore on`）、[[in-sight]]
- 词源要点：**wear on**＝wear（古英语 werian「穿、佩戴」← 原始日耳曼语 \*wazjanan ← PIE \*wes-「穿衣」，同族 vest / invest / vestment）＋ on（小品词，表「继续下去」，同 go on / read on / carry on）——「时间继续往前磨」；**in sight**＝in + sight（古英语 sihþ ← 日耳曼语 \*sihtiz ← PIE \*sekʷ-「看」，同族 see / insight / foresight / hindsight）；「临近、在望」义＝目标进入视野的隐喻（故 no end in sight ＝ 尽头不在视野里）
- 易混要点：**wear + 小品词四兄弟** on / off / out / down（含义相去甚远）；**in sight vs insight（只差一个空格！）**、on sight（一见就，shoot on sight）、out of sight、at sight；各登记一条词族候选
- **备注（环境限制）**：出网仍被 sinkhole（复测解析仍为 198.18.0.0/15），两词细节标「待验证」；待回补清单累计 **7** 条
- 更新：[[english-vocabulary]]（进度 15→17、结构约定加多词条款、两条词族候选）、`index.md`（Stats：vocab 15→17）、`CLAUDE.md`（v1.5）
- 另注：根目录出现 `Untitled.canvas`（2 字节，内容 `{}`）——Obsidian 空画布残留；未删除（不静默删用户文件）、未入库，待用户定夺

## [2026-10-01] schema | v1.6 法语词汇系统 + vocab 目录分语言重构（en / fr）

- 触发：用户决定新增**法语**词汇系统（沿用 inbox 收词；条目深度＝阴阳性 + 复数 + 动词变位要点；词源对照 CNRTL），并把词汇目录按语言分轨
- 重构：`git mv` 17 条英语词条 `wiki/vocab/*.md` → `wiki/vocab/en/`（**纯重命名、内容零改动**，独立提交 3ebd3ac，`git show --numstat` 验证 0 插入/0 删除）；`.gitkeep` 迁至 `wiki/vocab/fr/`。wikilink 按 basename 解析（`newLinkFormat: shortest`）——迁移前后 unresolved-link 基线一致（仅 `templates/vocab.md` 的 `[[other-word]]` 一处真断链，本次一并修复为反引号）
- 新增：`templates/vocab-fr.md`（`lang` / `gender` / `plural` 字段 + 「形态要点」章节；动词不列表格）、hub 页 [[french-vocabulary]]（含命名消歧登记表）、收件箱 `raw/vocab-fr-inbox.md`
- schema 同步（CLAUDE.md）：§1 树（vocab/en、vocab/fr）、§2.1（`lang` + `gender` / `plural`）、§2.2（非 ASCII 文件名转写）、§3.4（语言分轨对照表：inbox / 目录 / 模板 / hub / log 标记 `vocab-fr:` / 权威源 CNRTL；法语命名与消歧细则）、§4（双 hub + Stats 格式）、§7（v1.6）
- 词条变更：17 条英语词条补 `lang: en`；闪卡小节标题 `#flashcards` → `#flashcards/en`（语言子牌组，与法语 `#flashcards/fr` 对称；据插件源码子标签自动纳入；排程零风险——迁移前全库无 `SR:` 注释、插件 cardSchedules 为空）
- 幽灵卡修复：模板不再带 `#flashcards` 标签（SRS 插件无模板目录排除，`templates/vocab.md` 的占位卡此前可能被解析为幽灵卡；两个模板均改为注释说明 + 实例页带标签）
- 命名规则要点（法语）：文件名 ASCII 转写（é→e、ç→c、œ→oe、撇号→连字符）；**全库唯一**——跨语言同名加 `-fr`（如 décor → `decor-fr.md`，与 [[decor]] 互链）、同语言近形预置词性后缀（`ou-conj` / `ou-adv`）、避开 Windows 保留名（con / aux / nul）；lemma 取 headword（`suis` → `être`）
- 待办/风险：① 法语词源核对债务随首批词条起累积（出网被 sinkhole 复测确认，一律标「待验证」，登入 [[french-vocabulary]] 待回补清单）；② 幽灵卡修复需在 Obsidian 中核实牌组数（英语应为 17 词条 / 34 卡）；③ `raw/vocab-inbox.md` 内「→ wiki/vocab/」为历史文本，按 §5 规则 1 不改
- 另：根目录 `Untitled.canvas`（空画布残留）经用户确认删除；该文件未入库，删除不影响 git 历史
- 更新：`index.md`（Topics 双 hub、Stats：wiki 页面 16→17、vocab 含 en/fr 分计）、[[english-vocabulary]]（路径、闪卡标签、多词待办句移除、法语互链）、[[personal-knowledge-management]]（页面地图 + 开放问题）

## [2026-10-01] ingest | vocab-fr: 首批 1 词（gouvernement）——法语系统首条词条

- 来源：`raw/vocab-fr-inbox.md`（Duolingo）→ src/duolingo；去重：无既有词条
- 新建词条 1 条：[[gouvernement]]（法语系统首条，`templates/vocab-fr.md` 首次实装）
- 词源要点：gouverner + -ment（拉丁 -mentum）← 拉丁 gubernare「掌舵、治国」← 希腊 kybernan「掌舵」——与 gouvernail（舵）、gouverneur、英语 govern / governor / cybernetics 同源（「治国＝掌舵」隐喻）
- 易混要点：le gouvernement（内阁/行政班子）vs l'État（国家，更广）、la gouvernance（治理）、le régime（政权）；拼读 gou=/ɡu/、-ment=/mɑ̃/
- **备注（环境限制）**：出网仍被 sinkhole，CNRTL 不可达——「12 世纪首见」等年代细节标「待验证」，登入 [[french-vocabulary]] 待回补清单（法语核对债务开账，共 1 条）
- 更新：[[french-vocabulary]]（进度 0→1、待回补开账、新增「掌舵」词族候选）、`index.md`（Stats：fr 0→1）
- 首批观察：v1.6 命名规则（ASCII 转写 / 全库唯一 / Windows 保留名）均未触发消歧——`gouvernement` 无近形与同名冲突

## [2026-10-01] schema | v1.6.1 法语词条新增「英法交叉核对（受影响的英语词）」

- 触发：用户请求——法语词条要找出**受该法语词影响的英语词**做 cross-check
- 新规：法语词条新增「**英法交叉核对（受影响的英语词）**」小节（每词条必做）——分清**借入**（英 ← 法：路径、年代、拼写/读音漂移、词义异同）与**共祖**（同源非借入，勿混）；反向回借（法 ← 英）可留意；英语词库已收录则双向互链；无网络标「待验证」
- 同步：`templates/vocab-fr.md`（新小节 + 闪卡背面「英法交叉」行）、`CLAUDE.md` §3.4 法语要求 + §7（v1.6.1）、hub [[french-vocabulary]]「概述」「结构约定」
- 实装：[[gouvernement]] 新增该小节——英 government / govern / governor / governance ← 古法语本词族（拼写丢 u、词义更宽）；governess 为平行构词；cyber- 共祖非借入；另记「gouvernance 1990 年代从英语回借」待验证
- 备注：英语词库暂未收录 government 等同源词——如后续收词，两页互链；核对债务（CNRTL / etymonline）仍待联网
- 更新：`log.md` 本条；词条 `updated` 保持 2026-10-01（同日多次修订）

## [2026-10-01] ingest | vocab-fr: 第二批 4 词（cathédrale / pâtissier / chapitre / détective）

- 来源：`raw/vocab-fr-inbox.md`（Duolingo）→ src/duolingo；去重跳过 gouvernement
- 新建词条 4 条：[[cathedrale]]、[[patissier]]、[[chapitre]]、[[detective]]——v1.6.1「英法交叉核对」首次批量实装
- 词源要点：**cathédrale**＝拉丁 cathedra「座椅」（← 希腊 kathedra）——主教之座所在之堂；**pâtissier**＝pâte（← 拉丁 pasta ← 希腊 pastē 麦粥/面团）+ -ier；**chapitre**＝拉丁 capitulum「小头」（caput 的指小）——教会「首脑议事会」义由此；**détective**＝**反向回借**（法 ← 英，约 1907）：英语 detective ← detect ← 拉丁 detegere「揭开」= de- + tegere（盖）
- 英法交叉要点：**chair 与 cathedral 同为拉丁 cathedra 的双重借入**；**chief / chef** 同为英语对同一词源的双重借入（caput 家族）；paste / pasta / pâté / pastry 同出拉丁 pasta；detect（揭盖）与 protect（盖住）同根反义
- 异常处理：inbox 首行 `cathedrate`——法语无此词，判定为 **cathédrale** 的拼写变体（**待用户确认**），按 lemma 编译并在词条加「收录说明」；`patissier` / `detective` 为缺重音形式，lemma 按标准拼写（pâtissier / détective）
- 规则细化：跨语言同名消歧改为「**后建页者**加语言后缀」（原规则只写法语 `-fr`；因 [[detective]] 先建、英语 detective 可能后建，补齐对称情形 `-en`）——同步 §3.4 与 hub 结构约定；[[detective]] 预登入 hub 命名消歧登记表
- 更新：[[french-vocabulary]]（进度 1→5、待回补 5 条、词族候选 ×4：caput / cathedra / pasta / 英语回借簇）、`index.md`（Stats：fr 1→5，总 22）
- **备注（环境限制）**：出网仍被 sinkhole（复测 cnrtl.fr 解析仍为 198.18.0.128），年代/路径细节逐条标「待验证」

## [2026-10-01] ingest | vocab-fr: 第三批 7 条（il y a / environ / mot / nouveau / auteur / connaître / court）

- 来源：`raw/vocab-fr-inbox.md` → src/duolingo（本批 7 条 inbox **未标注出处**——推断 Duolingo，**待确认**）；去重跳过 5 条（gouvernement / `cathedrate`→cathédrale / patissier / chapitre / detective）
- 新建词条 7 条：[[il-y-a]]、[[environ]]、[[mot]]、[[nouveau]]、[[auteur]]、[[connaitre]]、[[court]]——含法语系统**首条多词单位**（il y a → `il-y-a.md`）
- lemma / 异常处理：`neuveaux` 非法语词 → 判定为 nouveaux（nouveau 阳复）笔误、归并 [[nouveau]]；`connais`（je/tu 现在时）→ 不定式 [[connaitre]]；`court` 有歧义（形容词「短的」/ 网球场 / courir 第三人称）→ 按最可能教学词「短的」编译、另两义在易混中说明——均加「收录说明」并待用户确认
- 词源要点：**il y a**＝il + y（拉丁 ibi「那里」）+ a（avoir ← habere）＝「那里有」——与英 there is 语法化路径同构；**mot**＝拉丁 muttum「嘟囔声」→ 法语把「一声嘀咕」抬成「词」，verbum 分化为 verbe/parole；**nouveau**＝拉丁 novellus（novus「新」指小）；**auteur**＝拉丁 auctor ← augere「增加」——「使作品生长出来的人」；**connaître**＝拉丁 cognoscere ← PIE \*gneh₃-（与英 know 同源）；**court**＝拉丁 curtus「截短」← PIE \*(s)ker-「切」
- 英法交叉要点（本批亮点，借入 vs 共祖样本）：**new（共祖）vs novel（古法语借入）**；**know / can（共祖）vs connoisseur（法语借入，1714）**；**auteur / author 双重借入（doublet）**；**environs / environment ← 法语**；**bon mot / mot juste ← 法语**；**curt 拉丁直借 vs court 经古法语（借的是 cour，另一词源）**
- 更新：[[french-vocabulary]]（进度 5→12、待回补 12 条、词族候选 ×3：connaître/savoir、court·cour·cours、nouveau·neuf·récent+new/novel）、`index.md`（Stats：fr 5→12，总 29）
- **备注（环境限制）**：出网仍被 sinkhole（复测 cnrtl.fr 解析 198.18.0.132），法语词源核对债务累计 **12** 条，待联网一并清账

## [2026-10-01] ingest | vocab-fr: 第四批 14 条 → 12 词条（présentateur / certain / trouver / émission / découvrir / incroyable / déjà / voir / littérature / moderne / prendre / ça）

- 来源：`raw/vocab-fr-inbox.md` → src/duolingo（本批 inbox **未标注出处**——推断 Duolingo，**待确认**）；去重跳过 12 条（前 12 均已编译）
- 新建词条 12 条：[[presentateur]]、[[certain]]、[[trouver]]、[[emission]]、[[decouvrir]]、[[incroyable]]、[[deja]]、[[voir]]、[[litterature]]、[[moderne]]、[[prendre]]、[[ca]]；另补 [[auteur]]（并入 `autrice`）
- **lemma / 归并**：`presentateur` + `presentatrice` 合并为 [[presentateur]]（-teur/-trice 阴性格配，一页承载）；`autrice` 并入 [[auteur]]（阴性形式）；`trouve`→[[trouver]]、`decouvert`→[[decouvrir]]、`vu`→[[voir]]（动词变位形式归不定式）；`deja`/`emission`/`litterature` 补重音、`ca` 补软音符
- 词源要点：**trouver**＝通俗拉丁 *tropare「作诗寻章」← 希腊 tropos——「找到」由「寻得妙句」泛化；**émission**＝拉丁 emittere（ex- 出 + mittere 送）——「节目」义英语没有（**faux ami**）；**déjà**＝des ja（dès + 拉丁 jam「已经」）——与 jamais 同根反义；**voir**＝拉丁 videre ← PIE \*weid-（看/知一体两面）；**prendre**＝拉丁 prehendere「抓」——一「抓」串起拿/吃/乘/花五义；**ça**＝cela 的口语缩合
- 英法交叉亮点：**trove**（= 法语 trouvé）← treasure trove；**discover / cover / covert** ← 古法语 descovrir/covrir；**déjà vu** ← 法语（1903）；**view / voyeur** ← 法语 veoir/voir；**entrepreneur / surprise（-prise = 抓）**；**modern ← 法语 moderne**
- 更新：[[french-vocabulary]]（进度 12→24、待回补 24 条、词族候选 ×3：prehendere「抓」、videre「看/知」、credere「信」）、`index.md`（Stats：fr 12→24，总 41）
- **备注（环境限制）**：出网仍被 sinkhole（复测 cnrtl.fr 198.18.0.132、etymonline 198.18.0.112），法语词源核对债务累计 **24** 条

## [2026-10-01] ingest | vocab-fr: 第五批 15 条 → 14 词条（mon / frère / chercher / son / déodorant / pouvoir / jamais / rester / longtemps / souffle / bougie / gâteau / sur / inacceptable）

- 来源：`raw/vocab-fr-inbox.md` → src/duolingo（本批 inbox **未标注出处**——推断 Duolingo，**待确认**）；去重跳过 24 条
- 新建词条 14 条：[[mon]]、[[frere]]、[[chercher]]、[[son]]、[[deodorant]]、[[pouvoir]]、[[jamais]]、[[rester]]、[[longtemps]]、[[souffle]]、[[bougie]]、[[gateau]]、[[sur-prep]]、[[inacceptable]]
- **lemma / 归并**：`frere`/`gateau`/`deodorant` 补重音；`cherche`→[[chercher]]、`peut`→[[pouvoir]]（动词形式归不定式）；`son`（主有/声音）与 `souffle`（气息/动词形）为同形双义，各并一页；`sur` 按命名消歧规则记 `sur-prep.md`（与 sûr 近形对，登入命名登记表）
- **句子处理**：inbox 出现整句 `elle ne peut jamais y rester longtemps`——非 headword，不建页，拆解为 pouvoir/jamais/y/rester/longtemps（均已收），并在 [[rester]] / [[longtemps]] 词条中作为例句复用
- 词源亮点：**chercher**＝晚期拉丁 circare「绕行」← circum「环绕」——「找」＝绕圈巡视，英 search 即其借入；**bougie**＝地名 Béjaïa（蜡烛贸易港）→ 通名，英俚语 bougie（← bourgeois）与之**无关**（词源陷阱）；**gâteau**＝古法语 gastel ← 法兰克语（法语里少见的日耳曼词），与英 cake 不同源；**rester**＝re- + stare「站」→ 英 rest/arrest 借入；**pouvoir**＝拉丁 posse（pot- + esse）→ 英 power 借入；**son**（声音）→ 英 sound
- 英法交叉亮点：**sound ← 古法语 son**；**friar ← 古法语 frere**（vs brother 共祖）；**search/research ← 古法语 cerchier**；**déodorant ← 英 deodorant（反向回借）**；**jamais vu ← 法语**（déjà vu 的反面）；**inacceptable（法 in-）vs unacceptable（英 un-）平行构词**；**soufflé / gateau / surface / power 各为其法语源词的英语借入**
- 更新：[[french-vocabulary]]（进度 24→38、待回补 38 条、命名消歧登记表 +sur-prep、词族候选 ×3：capere「拿」、主有词 mon/ton/son、stare「站」）、`index.md`（Stats：fr 24→38，总 55）
- **备注（环境限制）**：出网仍被 sinkhole（复测 cnrtl.fr 198.18.0.132），法语词源核对债务累计 **38** 条

## [2026-10-01] setup | LLMLangWiki 建库（自 LLMWiki 拆分）

- 触发：用户决定将英 / 法词汇系统自 LLMWiki 整体迁出为独立语言专库（单一维护点；原库同日 v1.7 条目对应）
- 迁入（逐字节复制 + SHA256 全量校验一致）：`wiki/vocab/en/`（17 条）、`wiki/vocab/fr/`（38 条）、hub [[english-vocabulary]] / [[french-vocabulary]]、`templates/vocab.md` / `vocab-fr.md` / `topic.md` / `answer.md`、`raw/vocab-inbox.md` / `vocab-fr-inbox.md`、`output/2026-10-01-vocab-quiz-01.md` + `output/README.md`
- 基础设施：`.obsidian/` 配置（附件目录 `raw/assets`、模板目录 `templates`、快捷键 `Ctrl+Shift+D`）+ SRS 插件 v1.15.4（含本体 main.js / styles.css，gitignored）；`.gitignore` / `.gitattributes` 同原库；不含 workspace.json（全新工作区）
- 新写：本库 `CLAUDE.md`（词汇专用 Schema v1.0，fork 自 LLMWiki v1.6.1；§7 载继承的词汇 schema 演进摘要）、`index.md`
- 复核：SRS `dataStore=NOTES`——排程写于词条笔记 `<!--SR:…-->` 注释、随文件迁移（全库当前 0 条，尚未开刷）
- 待办：① 联网后清账两语「待回补」清单（en 7 条 etymonline ｜ fr 38 条 CNRTL）；② Obsidian 打开本库核对 graph 与 SRS 牌组树（flashcards/en、flashcards/fr）

## [2026-10-01] ingest | vocab-fr: 第六批 11 条（y / physique / bancaire / membre / lettre / recevoir / vouloir / en parler / parler / aujourd'hui / la）——法语 inbox 积压清零

- 来源：`raw/vocab-fr-inbox.md` 末 11 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 41 行（含第五批句子拆解）均已编译，无重复
- 新建词条 11 条：[[y]]、[[physique]]、[[bancaire]]、[[membre]]、[[lettre]]、[[recevoir]]、[[vouloir]]、[[en-parler]]、[[parler]]、[[aujourd-hui]]、[[la-art]]——法语系统首见：代词词条（y）、多词表达独立页（en parler）、冠词词条（la）
- **lemma / 归并 / 命名**：`veux`（现在时）→ 不定式 [[vouloir]]；`en parler` 按 v1.5 多词单位约定独立建页（parler + 代词 en 的高频组合）；`la` 按命名消歧「预置词性后缀」规则记 **`la-art.md`**（与 là 近形对——日后收 là 记 `la-adv.md`），已登入 hub 命名登记表；`la` 的 inbox 语境不明（定冠词 / 代词 / 实为 là）——按最高频（定冠词）编译并附代词义，词条与 hub 均标**待用户确认**
- **重大环境变化（本批关键）**：CNRTL / etymonline **直连仍被 sinkhole**（复测 cnrtl.fr → 198.18.0.128、etymonline.com → 198.18.0.134），但 **WebSearch 通道可用**——本批词源改走「**检索转引** CNRTL / TLFi / Académie 资料」核对（首个不以「待验证」整体开账的批次）：
  - y：842 年《斯特拉斯堡誓词》形式 **iv**「那里」（CNRTL y 词条，据 FEW）；**1208 年 il y a**（Villehardouin）；ibi vs hic 之争两说并列（CNRTL 倾向 hic，因 ibi 语音困难）
  - vouloir：TLFi 记民间拉丁 **\*volere**（古典 velle 按 volui 类推重构）；最早文献（墨洛温 / 9 世纪）来源不一——标待验证
  - recevoir：拉丁 **recipere**（re- + capere）；10 世纪 recivre（《圣莱热传》）→ 1080 recevoir（《罗兰之歌》）；recette / reçu / récipient 同族
  - parler：教会拉丁 **parabolare**（678–79 / 853 年记录）；法语首见约 **1200**（《Aiol》，TLFi）
  - aujourd'hui：**hodie < hoc die**；hui / oi 10 世纪 → 12 世纪强化 → 14 世纪凝固（Grevisse / CNRTL 转引）
  - membre：拉丁 membrum，11 世纪借入（Académie 9e）；「成员」义 1200 年（Le Robert 史料）
  - lettre：拉丁 littera；10 世纪后半《圣莱热传》首见；语义线至 homme de lettres（1580）
  - physique：希腊 phusis → 古法语 fisique 1165 → 现代义 1708；la physique（f.）/ le physique（m.）
  - la：拉丁 illa（宾格）→ 9 世纪（Académie / CNRTL「LE」条）
  - en：← 拉丁 inde（Grevisse 转引）；首见年代未获——标待验证
  - **banque / bancaire：检索未获 CNRTL 数据**——首见年代标待验证
- 英法交叉亮点：**will 与 vouloir 同出 PIE \*wel-**（跨语系共祖，与 volition / volunteer / benevolent 的拉丁共祖线并列）；**receive（约 1300 ← 古法语）+ receipt 双借入**；**parley / parliament / parlor / parole ← parler 词族**；**letter / literature ← littera**；**today（OE tō dæge）与 aujourd'hui 结构平行**；**bank（河岸，古诺斯语）vs bank（银行，经法语 / 意语）同形异源**；**à la 入英（16 世纪末）**
- **回补既有词条**：[[il-y-a]] 补 CNRTL 核实的 **1208 年首见**（Villehardouin）并互链 [[y]]；[[rester]] 的 `y rester` 搭配补链 [[y]]——第五批句子 `elle ne peut jamais y rester longtemps` 至此全部成分建页完毕
- **日志更正（append-only）**：第五批条目称句子拆解「均已收」——其中 **y 当时未建页**（本次补建）；另「预置词性后缀」在 la 上首次实装（`la-art.md`）
- 更新：[[french-vocabulary]]（进度 38→49、积压清零、待回补 38 条不变 + 第六批不入账说明、命名登记表 +la-art、词族候选 ×5（littera / velle / y-en / parler / banca）、capere 候选更新）、`index.md`（Stats：fr 38→49，总 55→66；Backlog 记检索通道发现）
- **待办（向用户）**：① 存量 38+7 条待回补可改走 WebSearch 转引批量清账（**待确认**）；② `la` 的 inbox 本义（冠词 / 代词 / là）**待确认**；③ 第三至六批来源（推断 Duolingo）**待确认**

## [2026-10-01] ingest | vocab-fr: `la` 确认为宾语代词——拆页 [[la-pron]]（与 [[la-art]] 并存）

- 触发：用户确认 inbox 的 `la` 为**直接宾语代词**用法（je la vois 类）——第六批曾按最高频（定冠词）编译于 `la-art.md` 并标待确认
- 新建 [[la-pron]]：宾语代词全貌——位置（动词前 / 复合过去助动词前 / 否定包夹 / 命令式后置）、省音 l'、**COD 前置的分词配合**（je l'ai vue）、代词叠用顺序（me/te/se → le/la/les → lui/leur → y → en）、la vs lui；词源与冠词同出拉丁 illa（宾格 illam，9 世纪——CNRTL/Académie 经检索转引）
- [[la-art]] 调整：收录说明改为「用户确认宾语用法 + 代词详页指向 [[la-pron]]」；闪卡改为纯冠词卡（正面 `la（冠词）`），与 [[la-pron]] 的代词卡（正面 `la（代词）`）分工，防 SRS 同面混淆
- hub [[french-vocabulary]]：命名登记表行更新（la 按词性分页：la-art / la-pron；là → la-adv 待收）、词条数 49→50、待处理注记；`index.md` Stats（fr 49→50，总 66→67）
- 并行：同批已发 workflow「存量清账」（45 词，8 批子代理 + 1 核查代理）——结果与收尾见随后条目

## [2026-10-01] ingest | vocab-fr: `en` 建页——副词代词 [[en-pron]]（代词三件套：la-pron / y / en 齐）

- 触发：用户在 `la` 拆页后追加收词 `en`——按代词用法编译（`en-pron.md`）；介词 en（← 拉丁 in）同形异源，留 `en-prep.md` 待收，登记入命名表
- 新建 [[en-pron]]：代 de + 名词（j'en parle）与部分冠词 / 数量回指（j'en ai deux）；位置、否定、命令式（parles-en !）、固定搭配（en avoir besoin / s'en aller / en vouloir à）；词源 ← 拉丁 inde「从那里」（**9 世纪** int / ent，Académie——经检索转引；《罗兰之歌》已见今用，DMF 转引）
- 连带清账：[[en-parler]] 的「en 首见年代待验证」已核（9 世纪 int / ent）并补链 [[en-pron]]；[[la-pron]] 的 y / en 指向链接更新
- hub：命名登记表 +en 行、词条 50→51、候选「y / en 双璧」更新 + 新增「附着代词全景」候选；index Stats（fr 50→51，总 67→68）

## [2026-10-01] lint | 存量清账：fr 38 + en 7 词源待回补——8 批子代理「检索转引」核对（45 页更新，含多处实质订正）

- 触发：用户拍板「存量清账」并「fan out subagents」——workflow 派 8 批编辑子代理（fr 6 批 ×6–7 词、en 2 批 ×3–4 词）+ 1 只读核查代理；共 9 代理 / 404 次工具调用 / 92 万 token / 约 14 分钟
- 方法：CNRTL / etymonline 直连仍 sinkhole（复测 198.18.0.128 / 134），全程 **WebSearch 转引** CNRTL/TLFi/Académie（en：etymonline、医学词典）；只采纳检索明证、转引注明出处；无定论者保留「待验证」，不折中不编造
- 结果：**45/45 页更新**（核查 ok=true）——各页「词源/构词」「英法交叉核对行」「## 来源」三节最小改动；另 8 页闪卡卡背随实质订正同步（?? 结构完好）；frontmatter / 释义 / 例句未动
- **实质订正（旧稿矛盾/过时内容，符合显式记录原则）**：**détective** 法语借入实为 1871（Verne；原「约 1907」有误，闪卡同步）；**découvrir** 承晚期拉丁 discooperire（非「后世加 dé-」）；**gouvernement** governess 源自法语 governeresse（非英语自造）；**rester**「休息」义 rest 系日耳曼本族词、restaurant 出 restaurare ≠ restare（原「同家族」改正）；**gâteau** gâter 与 gâteau 非同源（原推测改正）；**présentateur** 英语 presenter 系自构词（非借自法语）；**court** curtail ← 古法语 courtault（非 court + -al）；**mot** bon mot 1735（原 17c. 改正）；**moderne / littérature / émission** 年代订正；**faucet** 理据改「两说并存」；**sew** 同源形式按 etymonline 校正
- 遗留 **15 项**真正无定论、如实保留「待验证」（明细在各词条与核查报告）：emission（节目义年代带 1906–1938 区间）、auteur（author -th- 机制两说）、inacceptable（capere 家族入英路径）、prendre（comprise / apprehend 路径）、environ（virer 两说；environment 入英路径）、connaitre（reconnoiter / ignore）、jamais（jamais vu 年代）、deja（Boirac 1876 转述）、litterature（alliteration 等）、in-sight（sight~sequi 假说）、faucet（fausset 两说）、endometriosis（首见 1927 vs 1925 两说）等
- 核查代理另报：① [[en-parler]] 漏改一行已补（en 首见 9 世纪已核）；② [[prendre]] prison / prize / surprise / reprisal 归类错标（原在「共祖」栏而描述为「经古法语借入」）——已改归「借入英语」并注订正；③ 13 页「待验证」仅为「无遗留」否定句（非债务）
- 更新：[[french-vocabulary]] / [[english-vocabulary]]（两边「待回补」清单 → 已清账）、`index.md`（Backlog）；无定论项清单存本条目
- 待办：第六批新页（lettre / vouloir / physique / membre / recevoir / bancaire / aujourd'hui / la-art / en-parler）与 [[counterweight]] 的少量细节、及上述可复检的残留项 → 第二批子代理复清（随后条目）

## [2026-10-01] lint | 存量清账·复清：16 词第二批子代理——最终仅余 9 项无定论（fr 6 ｜ en 3）

- 触发：第一批遗留（第六批新页细节 + 第一批未决项重检）——4 批子代理（R1–R4；132 次工具调用 / 32 万 token / 约 4 分钟）
- **未决项重检清除**：prendre（comprise early 15c ← 古法语 compris；apprehend late 14c / comprehend mid-14c 直源拉丁）；inacceptable（capere 家族全清：accept late 14c、conceive late 13c、capture 1540s、perception late 14c、receptive early 15c）；connaitre（ignore 1610s、reconnoiter 1707）；littérature（alliteration 1650s）；environ（**environment 系英语自造 c.1600——法语 environnement 现代义反而自英语回借（CNRTL）**，原「英语取自法语」之说订正）
- **第六批新页清除**：lettre（letter c.1200；littera 词源按 etymonline 记「不确定」——diphthera 说系 Watkins 说，争议本身获权威源佐证）；vouloir（最早 **881《圣厄拉莉颂歌》**；**velleity 订正：直接源自中世纪拉丁 velleitas（1610s），非经法语**——移入共祖栏并同步闪卡）；recevoir（receive c.1300；receipt 的 -p- 约 1500 后仿拉丁恢复）；parler（parley mid-15c / parliament c.1300 / parlor c.1300；paroler 争议按 Romania 1884 考订并记）；physique（英语再借入 **1826**）；membre（member c.1300）；**bancaire 三项全清**（banque 15 世纪、bancaire 19 世纪初、bank 金融义 late 15c）；aujourd'hui（today ← OE todæge；jour 家族年代；1220 au jor d'ui 证形）；la-art（à la late 16c；the ← 晚期 OE þe——另订正 **à la carte 实为 1826 入英**）；en-parler（en/y 语序史：约 1050「en i out」、17 世纪「en y」判废后「y en」通行）
- **英语侧**：[[counterweight]] 成词年代补入 **1693**（Merriam-Webster；Random House 1685–95）
- **最终遗留 9 项**（学界未定 / 无权威数据，如实保留「待验证」）：fr 6——auteur（author -th- 机制两说）、deja（Boirac 1876 转述）、emission（节目义年代区间 1906–1938）、environ（virer 词源 vibrare／\*gyrare 两说）、jamais（jamais vu 年代）、litterature（-litter-→-liter- 拼写简并年代）；en 3——in-sight（sight~sequi 假说）、faucet（fausset 两说）、endometriosis（首见年份 1927 vs 1925 两说）
- 另：各页「出处：推断 Duolingo——待确认」为**待用户确认项**（非检索可决），按规则原样保留
- 更新：[[french-vocabulary]]（清账行：两批 13 子代理、法语侧仅余 6 项）、[[english-vocabulary]]（counterweight 复清补入）、`index.md`（Backlog：仅余 9 项）
- 核对债务至此**基本清零**（余 9 项均属学界无定论类，非「未检索」类）

## [2026-10-01] ingest | vocab-fr: 第七批 25 条 → 25 词条（parfois / faire / marcher / tout / aller / appel / important / carrière / compte / salaire / être / choix / seul / classeur / amener / ouvrir / paire / précieux / brûlant / royal / morceau / casser / proche / dispute / produit）——法语 inbox 积压再次清零

- 来源：`raw/vocab-fr-inbox.md` 第 63–87 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 62 行均已编译，无重复
- 新建词条 25 条（4 批子代理并行编译 + 1 批重派）：`il fait`→[[faire]]、`un seul`→[[seul]]、`ouvert`→[[ouvrir]]（变位/形容词形式归并到 lemma，均写「收录说明」）；`carrière`/`être`/`précieux`/`brûlant` 补重音、文件名 ASCII 转写（carriere/etre/precieux/brulant）；本批无命名消歧冲突（无同名/近形/Windows 保留名）
- 词源要点（本批以高频核心词 + 英法交叉亮点为主）：
  - **faire**＝拉丁 facere ← PIE \*dʰeh₁-「放」；英 fashion←façon、feat←fait、feature←faiture、faction（借入），fact/factor/perfect（共祖）
  - **être**＝esse「是」+ stare「站」异干拼合；英 entity/essence/interest/absent/present←esse，is/am/are 为 PIE \*es- 共祖，be 出 \*bheu- 另根
  - **aller**＝vadere + ire + ambulare 三根异干；英 alley/amble/ambulance（借入）
  - **salaire**＝拉丁 salarium「买盐的钱」（士兵盐饷）← sal「盐」；英 salary 借入、salt 共祖（PIE \*sal-）
  - **choix**＝法兰克语 \*kausjan ← PIE \*geus-「尝、选」；英 choice 借入、choose 本族共祖
  - **morceau**＝拉丁 morsum「被咬的一口」← mordere；英 morsel 借入（英保留古形）、remorse 共祖
  - **royal**＝拉丁 regalis ← rex；英 royal 借入、regal 双重借入
  - **produit**＝拉丁 producere（pro- + ducere 引）；英 product/produce 借入、duke/conduct 共祖
  - **ouvrir**＝拉丁 aperire「揭开」；英 overt/overture 借入、cover 走反义 operire 侧
  - **amener**＝a- + mener ← 拉丁 minare「驱赶」；英 amenable ← 古法语 amener
- 英法交叉：本批英语对应词（fashion/feat/career/salary/choice/appeal/march/total/royal/morsel/approach/dispute/product/class/amenable/overt/pair/precious/sole/entity/essence 等）在 en/ 下均无页面，按规则普通文字提及、未建英语页；法语内部互链 [[rester]] / [[il-y-a]] / [[chercher]] 等
- **待回补**：法语侧首证年代普遍待 CNRTL/TLFi 详核（约 21 词具体定年）；另留 5 项学界未定如实保留：brûler 词源两说（ustulare / \*brodum）、marcher 终极来源、totus 来源不明、aller 的 aler 词干来源（ambulare 缩约 vs allari）、proche 俗拉丁中间形（propianus vs propius）
- **备注（环境限制）**：CNRTL 直连仍 sinkhole（web_fetch 仅返回「Portail lexical」空壳），本批词源全部 WebSearch 转引（etymonline / CNRTL-TLFi 检索明证）；批次 C 曾因 web_fetch 卡死失败（未写文件）——重派后改用纯 web_search 完成
- 更新：[[french-vocabulary]]（进度 51→76、词条列表全列、待回补 +25、词族候选 ×8：facere / esse / aller 三根 / pretium / sal / reg- / computare / solus）、`index.md`（Stats：fr 51→76，总 68→93；Backlog 词族候选约 28 + 第七批待回补）

## [2026-10-01] setup | 推送至 GitHub 远程（origin → github.com:cloudqiu/LLMLangWiki）

- 用户提供远程仓库 `git@github.com:cloudqiu/LLMLangWiki.git`；本库此前**无远程**（仅本地 `main` 分支）
- 添加 `origin` 远程并首次推送 `main`（含全部历史：建库 → 各批 ingest / lint 清账 → 第七批）
- **环境处理**：Git for Windows 自带 MSYS `ssh.exe` / `sh.exe` 在本 harness 沙箱下报 `couldn't create signal pipe, Win32 error 5`（命名管道受限）——改用 **Windows 原生 OpenSSH**（`C:\Windows\System32\OpenSSH\ssh.exe`）并经 **`GIT_SSH`** 直连（不经 MSYS shell，避开 `core.sshCommand` 的 shell 解释）后推送成功；SSH 身份 `id_ed25519`（cloudqiu）验证通过
- 备注：`GIT_SSH` 为会话级环境变量、非持久——后续在本 harness 内推送需重设（`$env:GIT_SSH = "C:\Windows\System32\OpenSSH\ssh.exe"`）；用户本机终端不受此沙箱限制，自带 MSYS ssh 应可用

## [2026-10-03] ingest | vocab-fr: 第八批 19 条 → 19 词条（hâte / avoir hâte de / école / solution / contrôler / rembourser / complètement / emporter / reine / micro-ondes / cuisine / postuler / aucune idée / intéresser / même si / loin / culture / payant / prince）——法语 inbox 积压再次清零

- 来源：`raw/vocab-fr-inbox.md` 第 88–106 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 87 行均已编译，无重复
- 新建词条 19 条（4 批子代理并行编译 + 主代理全批复核：结构自检 19/19 通过、链接全解析）：`interesse`→[[interesser]]（过去分词/形容词形式归并到不定式 lemma，页内写「收录说明」）；`hate`→hâte、`ecole`→école、`controler`→contrôler、`completement`→complètement、`meme si`→même si、`aucune idee`→aucune idée（补重音；文件名 ASCII 转写）；多词单位 3 条独立建页（[[avoir-hate-de]] / [[aucune-idee]] / [[meme-si]]）；本批无命名消歧冲突；6 个与英语同形词（hate / loin / solution / culture / cuisine / prince）登记入命名表（英语侧日后记 `-en`）
- 词源要点（全部 WebSearch 转引——CNRTL 直连仍 sinkhole）：
  - **hâte**＝法兰克语 \*haist「猛烈」（一说 \*haifst，并列）；名词首证 1165-70；英 haste late 13c 借自古法语——法兰克一源三分（英 haste / 德 Hast 反借 / 法语 hâter）
  - **avoir hâte de**＝1538 首见（Estienne）；英 can't wait 系独立形成（无借入）
  - **école**＝拉丁 schola ← 希腊 skholē「闲暇」；古法语 escole 11c；英 school / scholar 系拉丁→古英语直借（共祖），**scholastic 1590s 反而经法语**
  - **solution**＝拉丁 solutio ← solvere（se- + luere）；法语首证 1119「解释」；英 solution late 14c 借入（英「溶液」义 1590s 早于法语）
  - **contrôler**＝contre-rôle「对账簿」→「核查」；盎格鲁-诺曼 contre roller 约 1310；英 control 借「核查」义后英语侧漂移出「支配」义（mid-15c vs 16c 两说），20c 回流法语被 Académie 判 anglicisme——faux ami 重点页
  - **rembourser**＝re- + embourser（bourse「钱袋」← 希腊 byrsa）；首证 1262（一说 1444，并列）；英 reimburse 1610s ← imburse 1530s ← 法语 embourser
  - **complètement**＝complet（← completus，14c）+ -ment；英 completely early 15c 系英语自构（-ly）——平行构词非借入
  - **emporter**＝en-（← 拉丁 inde）+ porter（← portare）；首见约 980「enportet」；l'emporter 1350、s'emporter 1632；英 port / import / export / transport 承 portare；**important 法语系意大利语借词（首见 1476 / 1528，CNRTL）**
  - **reine**＝拉丁 regina ← rex；11c 首见；英 reign ← regnum（非 regina）、queen 与 reine 无词源关系（PIE \*gwen-）；同音三陷阱 reine / rênes / renne
  - **micro-ondes**＝micro-（希腊 mikros）+ onde（← unda）；20c 词；英 microwave 1931 英语自铸（是否互为 calque 待验证）；1990 拼写修正 microonde
  - **cuisine**＝coquina ← coquere；TLFi 首证 1155；**双重借入样本**（英 kitchen ← 古英语 cycene ← 通俗拉丁 \*cocina；英 cuisine 1786 自法语）
  - **postuler**＝拉丁 postulare「要求」；13c 首证；英 postulate 共祖拉丁（1530s ← 中世纪拉丁 postulatus）——faux ami（法＝应聘 ｜ 英＝假定）
  - **aucune idée**＝aucun ← aliquis + unus；古法语曾为肯定「某个」→ 语义反转「没有一个」（d'aucuns 化石；年代诸说并列不折中）；英 no / any / none 全为日耳曼来源、无关联
  - **intéresser**＝拉丁 interesse（intérêt 同源；1290 / 1356 首证）；经典易混 intéressant（有趣）vs intéressé（感兴趣）；英 interest mid-15c 借入，interesting / interested 系英语自构
  - **même si**＝même ← 通俗拉丁 \*metipsimus（ipse 家族）；même si + **直陈式**（≠ bien que + 虚拟式）；英 same 无词源关系（古北欧 samr）
  - **loin**＝拉丁 longe（longus 的副词形）；ca 1050 首证；英 loin「里脊」← 古法语 loigne ← lumbus——**同形假朋友**；英 long 共祖（PIE \*dlonghos-）
  - **culture**＝拉丁 cultura ← colere；12c「被耕种的土地」→ 17c「心灵培育」；英 culture mid-15c 借入（路径三源两说）；colony 同 colere 根
  - **payant**＝payer 的现在分词；payer ← 拉丁 pacare「安抚（债主）」；英 pay c.1200 ← 古法语 paier；「有利可图」义 1875 从英语回借（反向）
  - **prince**＝拉丁 princeps（primus + capere）；古法语 12c；英 prince c.1200 ← 古法语 prince；principal / principle 同出 princeps
- **待回补（≈ 20 项）**：英语侧派生词年代未逐一核（peace/pact/pacify、prime/primary、hasty、roll/role）；TLFi 原条未直连核对（payant / prince / culture / complètement）；首证年代未得（complètement 副词、micro-onde、idée、lointain、scolaire、「应聘」义、à emporter、\*kokina 入日耳曼年代）；并存两说如实并列（control 支配义 mid-15c vs 16c；solution 化学义 1676/1690；rembourser 1262/1444；Regina 首见；英 important mid-15c vs 16c；英 culture 路径）——明细在各词条「来源」节
- **连带清账**：[[important]] 回填（法语首见 1476 / 1528 补核 + 英语两说补记，原「待 TLFi 核实」项**已清**）；[[amener]] / [[royal]] 补互链（[[emporter]]、[[reine]]；均更新 frontmatter updated 2026-10-03）
- **备注（环境）**：CNRTL 直连仍 sinkhole（未 WebFetch，一律 WebSearch 转引）；子代理按检索**实质订正**三处主代理提示（英 control 支配义 mid-15c、英 culture mid-15c、英 postulate 动词源 postulatus）——均按检索记录并注明
- 更新：[[french-vocabulary]]（进度 76→95、词条列表全列、待回补 +第八批、命名表 +同形 6 词、词族候选 ×11：schola / contrôle·control / haste / solvere / longus / coquina / princeps / 否定词反转 / intéressant·intéressé / 钱簇 / portare）、`index.md`（Stats：fr 76→95，总 93→112；Backlog 更新）；随后 git 提交

## [2026-10-03] ingest | vocab-fr: 第九批 28 条 → 28 词条 + 2 综合页（roman / librairie / roi / princesse / savoir / bon / recette / tarte / citron / liseuse / raison / trop / petit…）——法语 inbox 积压再次清零，两个长期候选建页清账

- 来源：`raw/vocab-fr-inbox.md` 第 107–134 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 106 行均已编译，无重复
- 新建词条 28 条（5 批子代理并行编译 + 主代理全批复核：结构自检 28/28 通过、367 项链接全解析〔仅登记在案的 `[[grand]]` 占位〕）：归并——`fini`→[[finir]]、`sais`→[[savoir]]、`bonne`→[[bon]]、`offiirai`→[[offrir]]（疑 j'offrirai 手误）；拼写变体——`converture`→[[couverture]]（**待用户确认**）；多词单位 6 条独立建页：[[roman-policier]] / [[c-est-pour-ca-que]] / [[avoir-besoin-de]] / [[peut-etre]] / [[vu-que]] / [[marque-page]]
- **两个综合页建立（清长期候选）**：[[mots-de-la-royaute]]（「王室词汇」——rex 与 princeps **两条词根**的坑：roi / reine / royal vs prince / princesse）＋ [[connaitre-vs-savoir]]（认识 vs 知道辨析）；均登记入 index Topics 与 hub「已建」区
- 词源要点（全部 WebSearch 转引——CNRTL 直连仍 sinkhole）：
  - **roi**＝拉丁 rex/regem ← regere；首证 ca 880（rex，《圣欧拉莉颂歌》）；英 viceroy ← vice-roy（借入）、king（cyning ← \*kuningaz）零词源关系
  - **reine / royal**（rex 线）＋ **prince / princesse**（princeps ＝ primus + capere 线）——princesse ca 1175（`-esse` 法语本土派生）
  - **roman**＝中世纪拉丁 romanice「用罗曼语」——词源即「罗曼语」本身；古法语 romanz ca 1125；英 romance c. 1300 借入；英 novel 走 novellus 另线
  - **librairie**＝**faux ami 重点**：法语＝书店（中世纪曾＝「图书馆/藏书」→ 1360-70 义档起窄化）；英 library late 14c 借「图书馆」义后不再随法语漂移
  - **liseuse**＝lire 词族多义（1867 裁书刀 → 1889 小桌 → 1909 披肩 → 1918 阅读灯 → 1930 书封套；今主指电子阅读器）
  - **savoir**＝民间拉丁 \*sapēre ← sapere「有滋味→明智」；842 首见名词化；英 savoir-faire（1788/1815 两说）、savant 1719、savvy 1785（诸说）
  - **finir**＝拉丁 finire ← finis；古法语 fenir 约 1100 → 13c 依 fin 重构；英 finish（← 古法语 finiss-）/ fine / final / finance 借入
  - **offrir**＝拉丁 offerre；英 offer 双线（古英语 offrian 宗教义 ＋ 古法语 offrir 世俗义）
  - **bon**＝拉丁 bonus（10c buon）；bon vs bien ／ meilleur vs mieux 两套不规则；英 bonus 拉丁直借、bon appétit 1860 等短语借入
  - **recette**＝拉丁 recepta；英 receipt（← 古北法语 receite）与 recipe（拉丁祈使式）**同源双胞**；收入义 ↔ dépenses
  - **tarte** ca 1220；词源诸说并列；Tatin 传说与文献两说；**citron**＝法语「柠檬」↔ 英 lemon，英 citron ＝ 法语 cédrat（**双漂移**）
  - **trop** ca 1100（罗兰）；法兰克 \*throp「堆」说；英 de trop 1752 借入；**petit** ~980 ← \*pittittus；英 petty 贬化警示
  - **raison**＝拉丁 ratio；古法语 raizun ~980 首证（**初义「言语」**）；avoir raison；英 reason 借入、**raison d'être 1864 回借入英**
  - **généralement**（generalment fin 12e）、**probablement**（probable fin 14e）；**sans doute** 词义翻转三史料（1764→1798→1932）
  - **diplôme**＝希腊 diploma「对折文书」；**étagère** 经普罗旺斯语 estagiera 1379 借入；**marque-page** 20c 复合词（复数 marque-pages）
  - **couverture**＝cooperire（operire 侧）；首见 mil. XIIe s.；与 [[ouvrir]]（aperire）**反义根**；英 cover / coverture / curfew 借入、coverage 自构 1912
  - **bientôt**＝bien + tôt（1643 连写 Corneille——检索订正）；**donc** 980 dunc（\*dunc 主流说，三说并列）；**avoir besoin de**（besoin ← 法兰克 \*bisunni 说）
  - **vu que**（voir 过去分词语法化，首用约 1421 转引）；**c'est pour ça que**（口语引结果，与 vu-que 互为镜像）；**peut-être**（put cel estre 约 1120 → 1680 一说；与 peut être 最小对立对）
- 子代理按检索**实质订正**三处提示：king 词根（cyning ← \*kuningaz，PIE \*gene-）；英 roman「正体字」义 **1510s**（原记 1660s）；liseuse 英语借义（小桌 / bed jacket，非书封套）——均按检索记录并注明
- **待回补（≈ 24 项）**：英语侧细节（boon 两义合流、receipt 义变、petite 入英年代、diplomatic 年代、liseuse 英语借义）；TLFi 原条未直连 / 首证未得（trop、tarte、roi 拼写定型、étagère、tirer les rois、polar、电子阅读器义、librairie 现代义定型）；并存两说（tarte 词源、Tatin 传说、savoir-faire 1788/1815、librairie 窄化 16c vs 1846、princesse/principissa、donc 三说、peut-être 1680、polar 词源）——明细在各词条「来源」节
- **备注（环境）**：CNRTL 直连仍 sinkhole（未 WebFetch，一律 WebSearch 转引）；本批子代理 5 只 / 合计约 56 万 token / 并行约 7–8 分钟
- 更新：[[french-vocabulary]]（进度 95→123、词条列表全列、待回补 +第九批、命名表 +couverture / roman·liseuse 两行、综合页「已建」区 +2 并清 2 候选、词族候选 +8：书与阅读 / 厨房食谱 / bon·bien / recepta 双胞 / ratio / legere / 因果连接簇 / -ment）；`index.md`（Topics +2、Stats：fr 95→123，总 112→140，wiki 页面 2→4；Backlog 更新）；已 git 提交

## [2026-10-03] ingest | vocab-fr: 第十批 30 条 → 29 词条 + `ouvre` 补充（vrai / trésor / piment / dans / sauce / normal / mystérieux / heure / ensemble / magique / cacher / personnage / raconter / crevette / objet / entrée / passionnant / mère / qui / énerver / simple / fin / inventer / différent / courageux / blanc / bleu / ambiance / histoire）——⚠️ 本会话 WebSearch 预算耗尽，残留 ≈70 项登待回补

- 来源：`raw/vocab-fr-inbox.md` 第 135–164 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 134 行均已编译，无重复
- 新建 29 页 + `ouvre`→[[ouvrir]] 补充（同 lemma，frontmatter updated 2026-10-03）；归并：`blanche`→[[blanc]]、`bleue`→[[bleu]]、`differente`→[[different]]、`a neuf heures`→[[heure]]、`en entree`→[[entree]]、`cache`→[[cacher]]；拼写变体：`reconter`→[[raconter]]（待确认）；**判定待确认：`enserve`→[[enerver]]（s'énerve）**
- 词源亮点（部分经检索核实；预算耗尽后经 **etymonline 直读 + Wiktionnaire（引 TLFi/DMF）转引**补关键项）：
  - **vrai** ↔ 英 **very** ← 古法语 verai「真的」（late 13c 借入，mid-15c 才弱化为「非常」——faux ami 大彩蛋）；**trésor**（~1050 首证，圣阿莱克西斯传）↔ 英 **thesaurus**（1823 拉丁直借）——「三兄弟」同出希腊 thēsauros
  - **sauce** ← 拉丁 salsa ← sal（↔ [[salaire]]、saucisse/sausage 同线）；**piment** ↔ pigment 一源两枝（西 pimiento 线）；**normal** ← norma「矩尺」（école normale **1794** 立）
  - **qui**：**修正**——`qu'` 系 **que** 的省音（qui 元音前不省音）；who 与 qui 实为 PIE **\*kʷo-** 共祖（非「源异」）
  - **heure** ← hora（PIE \*yer-——与英 **year** 共祖）；英 hour c.1200 挤掉 OE tīd；inbox `a neuf heures` 归并入页（链 [[ouvrir]]）
  - **énerver**：拉丁 enervare「抽筋→使虚弱」→ **1836** 翻转「使烦躁」；英 enervate 守「使虚弱」旧义——同源异义 faux ami；**énervé ≠ nervous** 干扰点
  - **cacher** ← VL \*coacticare ← cogere；英 **cache 1797** 借入（后成计算机术语）、cachet 同族双胞；inbox `cache` 三形均在页内说明
  - **histoire**：history（拉丁线）/ story（法语线）——同一希腊词**两次入英**，1500s 才分工（双借入彩蛋）
  - **courageux** ← courage ← cœur（cor）——「勇气＝心气」；英 courage/courageous 借入；**Courage ! ＝加油**（faux ami）
  - **blanc / bleu** ← 法兰克 \*blank / \*blao（日耳曼借层；英 blank 13c、**blue c.1300 皆经法语**）；同层互链 [[hate]] / [[gateau]] / [[choix]]
  - **mystérieux** 首证 **前 1475**（**修正提示「17c 词」**）；inventer ← invenire「碰见→发现→发明」（invent/inventory 同源）；raconter ← re- + aconter ← computare（↔ [[compte]]）；dans ← deintus（~1112）；crevette ← 诺曼 chevrette「小山羊」（1532）；piment/objet/mère/qui 等拉丁链已核
- **环境（重要）：本会话 WebSearch 上限 200 次于本批中途耗尽**——主代理向 7 个在跑代理广播止损（不重试、未核项标「待验证」+注明原因）；替补通道 **etymonline 直读 / Wiktionnaire 转引** 经多代理验证可行
- 子代理按检索实质修正 4 处提示：qu'=que 省音；who/qui 共祖；mystérieux 15c（非 17c）；英 ensemble 音乐义 1844 / 套装 1927（非 1703/1774）
- **待回补（≈ 70 项）**：①法语各词首证年代未得 ≈20 词（vrai / sauce / normal / mystérieux 前 1475 系年 / ensemble / magique / cacher / personnage / entrée / passionnant / mère / simple / fin / inventer / différent / courageux / bleu / ambiance / histoire 等）；②英语衍词年代零星；③并存两说（simple simplus/simplex、fin 形容词两源、mysterious 路径、thēsauros 深层、cacher 法兰克说待考）；④判定待确认（enserve / reconter）——明细在各词条「来源」节
- 更新：[[french-vocabulary]]（152、词条列表全列、待回补 +第十批、命名表 +énerver、候选 4 更新【书与阅读 / 厨房食谱 / sal / computare】+ 4 新增【cœur·cor / 法兰克借词层 / hora / PIE \*kʷo-】、开放问题 +检索预算注）、`index.md`（Backlog +2 行、Stats：fr 123→152，总 140→184）；已 git 提交

## [2026-10-03] ingest | vocab: 英语第二批 15 条 → 15 词条（orderly / scrub / gaunt / stripe / linoleum / fluorescent / sterile / shiver / flicker / stiff / menacing / close-up / bursting / halftone-dot / cyan）——英语轨首次扩容（17→32）

- 来源：`raw/vocab-inbox.md` 第 26–40 行（前 9 条标注「From one prompt」；后 6 条未标注——推断同源，**待确认**）；去重：前 25 行均已编译，无重复
- 新建 15 页（3 批子代理并行）；多词词条 2 条（[[close-up]] **1913**、[[halftone-dot]]——按 v1.5 多词约定）
- 已核实要点：**fluorescent** ← fluor（拉丁 fluere）+ -escent，**1852 G. G. Stokes 命名**；**sterile** 细菌学义 **1877**（PIE \*ster- 两说）；**menacing** ← minacia（PIE \*men-「突出」——minae 原义「城墙尖角」）；**close-up** 1913（OED）；**burst** ← 古英语 berstan（-st- 换位史：bresten→burst）；**orderly** 1570s；**stripe** ← 中古荷兰 strīpe（词根两说）；scrub 同形两源（擦洗 / 灌木——两说不折中）；gaunt 词源**三说并列**（无定论）
- **环境**：WebSearch 预算耗尽——**linoleum / cyan 全条未核**、**shiver / flicker / stiff 词源未核**、halftone-dot 1873 未确证等；残留 **≈ 17 项**登 [[english-vocabulary]] 待回补（含出处「推断同源」待确认）
- 更新：[[english-vocabulary]]（32、词条列表全列、【其中 4 条多词词条】、待回补 +第二批、候选 +2：印刷 / 色彩术语簇、医院场景词簇）；`index.md`（Backlog +1 行、Stats：en 17→32）；已 git 提交

## [2026-10-03] query | 答案归档：[[verbes-pronominaux]]（法语代词式动词为什么存在？）——raw/ 文档整理入 wiki/answers/

- 触发：用户将一份问答文档放入 `raw/法语反身代词（代词式动词）为什么存在？.md`（raw/ 只读不改）——按「好答案回填」流程归档
- 新建 `wiki/answers/verbes-pronominaux.md`（type: answer）：四类代词式动词分工表（réfléchi / réciproque / essentiellement pronominaux / passif）、中文母语者四坑表（含复合过去一律 être + **分词配合规则**：se 为直接宾语→配合；间接→不配合）、速判口诀；**补上原文末尾许诺的「去掉 se 就换义」16 词对照表**（passer/se passer … emporter/s'emporter）
- 勘正：机制一节改述为「直承拉丁 se 用法、法语进一步规范化」（原稿「拉丁未做进词形」表述不确）；分词配合细节补全
- 互链：[[la-pron]]（叠用顺序）、[[en-pron]]（s'en aller）、[[emporter]] / [[interesser]] / [[hate]]（代词式用法）
- 更新：`index.md`（Answers 区 +1、wiki 页面 4→5）、log

## [2026-10-03] ingest | vocab-fr: 第十一批 1 条 → 1 词条（vie）——单条小批；清账通道验证：CNRTL/TLFi 原条全文经检索转引当场取得

- 来源：`raw/vocab-fr-inbox.md` 第 165 行 `vie`（**未标注出处**——推断 Duolingo，**待确认**）；去重：前 164 行均已编译；全库扫名（含 templates/）无 `vie`／近形冲突 → 直接建 `wiki/vocab/fr/vie.md`（n.f.，la vie，复数 les vies）
- 新建 1 页 [[vie]]（主代理直接编译，未派子代理——单条无需并行）
- **词源要点（法语侧经 WebSearch 转引 CNRTL/TLFi 原条，当场核实——CNRTL 直连仍失败，但检索摘要已能返回全文义项分层）**：
  - 拉丁 **vita**「生命」← **vivere**「être en vie」← PIE **\*gwei-**「活」；法语 **vie** 为裸词干，三字母原样承自拉丁
  - 字形演变：fin xe s. **vida / vide**（奥克语《Passion》）→ ca 1050 **vithe**（《圣 Alexis》*la vithe est fraisle*，-th- 为 10–11c. 抄本以希腊 θ 代 /t/ 的写法，同 [[auteur]] 页所记现象）→ ca 1100 **vie**（《罗兰之歌》*perdre sa vie*／*toute vostre vie*）
  - **义项链（TLFi 逐义项首证全取到）**：生命事实（10c/1050）→ 生命原理·活力 ca 1050 → 施于无生命物 1370（Oresme *un serf senz vie*）→ 来世·灵魂 life ca 1050、ca 1145（Wace *pardurable vie*）→ **人生** ca 1050/1150/1170（Chrétien de Troyes、Marie de France）→ **传记义** 1130-40（Wace *Dirai d'une virge la vie*）→ 时长 ca 1100／**à vie** 1288（贝阿芒瓦尔《博韦习惯法》）→ **生计义** 1395 *avoir sa vie*（＝avoir le vivre et le couvert）／1507-08（D'Amerval 洗牛谋生）／1676（Sévigné）→ **亲昵称呼义** 1561（Grévin *Mon bien, ma vie*）；TLFi 注（Ern. Meillet）：vie 仿希腊 βίος 学出「人生·人世」义
  - 形态教学点：*avoir / gagner / perdre / donner sa vie* 一律用 **sa** 不用 la；*à vie* 不带冠词且与 la vie 不同性（性别处理各家词典不一，**待验证**）；复数仅见 *avoir neuf vies*、*doubles vies*
  - 同族：[[vivre]]（未收：ca 1050《Alexis》*qui vivet*；现在分词 vivant → *de son vivant* 1489；名词化 **le vivre** ca 1160 → 复数 *les vivres*、1601 *couper les vivres*）、vital / vitalité、**vitamine**（1913 ← 英 vitamine 1912 ← 拉丁 vita + amine ← C. Funk）、**vieillir**（← vieil 即 vieux 旧形，**非**从 vie 派生）、envie（← invidia，另一 *vid-* 线）
- **英法交叉（英语侧 etymonline 直读，全条已核实）**：
  - **同义不同源（本日最大彩蛋）**：英语 **life** ← 古英语 lif ← 日耳曼语 **\*leiban** ← PIE **\*leip-**「黏附、附着」，与拉丁 vita **无亲缘**——法语 vie 与英语 life 是「同义异源」的一对（对照 [[loin]]↔long 的同形假朋友）
  - **经法语借入**：**vital**（late 14c. ← 古法语 vital ← 拉丁 vitalis；15c. 转「活着的」、1610s 转「至关重要的」）、**vitality**（1590s ← 拉丁 vitalitatem）
  - **不经法语**：**vitamin**（vitamine 1912 Funk 造，1920 去 -e 立 A/B/C 命名法）、**aqua vitae**（15c.「生命之水」＝酒精，对应法语 eau-de-vie）、**arbor vitae**（1660s，Lécluse 命名）、curriculum vitae
  - **反向回流（法←英←拉）**：法语 **vitamine** 1913 直接借英语（与 [[detective]] 同属「法语里的英语借词」样本）；**同形异源**：英语 **vie**（v. mid-15c.「争胜、下注」← OF envier ← 拉丁 invitare）与法语 vie **毫无关系**
- **待回补（≈ 4 项）**：法语 vital / vitalité / vieillir 首证年代；「à vie」的性别处理（各家词典不一）；古法语 **vite** ← 拉丁 vita 之说（Burdy 2019《Antico francese "vite" "modo di vivere" e la rima baciata》对该传统说法提出质疑）；Meillet 仿希腊说的原始文献
- **清账通道升级（可复用）**：新会话 WebSearch 额度可用，且检索摘要可返回 CNRTL/TLFi **原条全文**（义项分层＋逐条首证＋词源说明）——通道排序更新为 ①检索转引 CNRTL/TLFi 全文 ②etymonline 直读 ③Wiktionnaire（引 TLFi/DMF）转引；已写入 hub 开放问题
- 更新：[[french-vocabulary]]（152→153、词条列表 +[[vie]]、待回补 +第十一批、命名表 +vie 行（英语 vie-en 规则）、词族候选 +**vita「活」词族 + 一次语义分家**、开放问题 +预算重置验证）；`index.md`（Backlog +1 行、Stats：fr 152→153，总 184→185）

## [2026-10-03] ingest | vocab-fr: 第十二批 13 条 → 8 词条 + 2 处补充（comédie / ciné / désert / triste / compétition / équipe / quand / que penses-tu de；`a quelle heure`→[[heure]]、`finit-elle`→[[finir]]）——含「compétition 实为英→法回借」方向反转发现

- 来源：`raw/vocab-fr-inbox.md` 第 166–178 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 165 行均已编译；批内 `morceau`（166）、`casser`（170）系**重复**，`disupte`（172）系 `dispute` **手误**——均未重复建页
- 新建 8 页（3 批子代理并行，主代理做补充与记账）：[[comedie]]、[[cine]]、[[desert]]、[[triste]]、[[competition]]、[[equipe]]、[[quand]]、[[que-penses-tu-de]]（**多词短语页**，按 v1.5 约定：kebab-case 文件名、`word` 保留空格原形）；补充 [[heure]]（`a quelle heure` 问「某事的时刻」句式，与 Quelle heure est-il ? 分工）、[[finir]]（`finit-elle` 疑问倒装，含 -t- 由来与谐音 -t- 的区分）
- 词源亮点（法语侧经检索转引 TLFi / DMF / Académie 9e，英语侧 etymonline 直读，**当场核实为主**）：
  - **comédie** ← 拉丁 comoedia ← 希腊 kōmōidia（kōmos + ōidē「歌」）；首证 **1361**（Usito/NPR 转引，**待验证**）；英 **comedy late 14c. 经古法语 comedie**、comedian 1580s/c.1600 ← comédien、comedienne 1849；词源**三说并列**（传统「乡村歌」kōmē 说已被 Beekes 判「不再采信」，kōmos 来源不明／可能 Pre-Greek；另有 PIE \*komso- 一说）
  - **ciné** ← cinéma（TLFi 1899）← cinématographe（**1893 专利文本**已见）——阿波科普截短；首证 **1905**（*Photo-Ciné-Gazette* 刊名）／1908（*Ciné-Journal*），etymonline 记 1917——**两说并列不折中**；**造词者两说**：Lumière 兄弟 1895 vs **Léon Bouly 1892 *Cynématographe***；1895-12-28 大咖啡馆首次公开放映；英 **cinema 1899 借自法语 cinéma** → 英再截 `cine`（1928，一说部分借自法语）——「**法语造词 → 英语借入 → 英语再截短**」镜像；kinetic 等系希腊直入（共祖）
  - **désert** ← 帝国拉丁 **desertum**（名词）／古典拉丁 **desertus**（形容词）← deserere（de- + serere「联结」）；《罗兰》**ca 1100**（Bédier 校 v. 3246 作形容词「inculte」vs Gautier 词表作名词 ← Desertum）vs fr.wiktionary **1080**——并存；英 desert n. c.1200 经古法 desert ← 晚期拉丁 desertum、adj. mid-13c.、v. c.1600 ← déserter、just deserts（**另一线** ← deservire）；**dessert ↔ désert 反向词源**：dessert ← desservir ← servire（法语 1393 *desserte* → 1549 *dessert*）——一字母两词源的标本
  - **triste** ← 拉丁 **tristis**（**词源不明**，de Vaan 未列日耳曼同源）→ 古法语 **trist**（10c., 约 950-1000）→ triste ca 1145；tr **tristesse ca 1165**（Benoît de Sainte-Maure《Troie》16028，+ -esse 仿拉丁 tristitia，非继承）；英 **trist early 15c.**（← 古法 triste）与 **triste late 18c.**（再借义「乏味」）双重借入（en.wiktionary 标 rare）、tristful late 15c.；**勘正两处**：德语 **traurig 非亲缘**（← 日耳曼 \*drūrēn，与英 **dreary** 同源；与法语 triste 亲缘的是 18c. **借**自法语的德 trist）；**Tristan ← 威尔士 Drystan**（受 triste 民间词源影响，非真词源）；英 **tryst** 系同音假朋友
  - **compétition**——**方向反转的发现**：法语 compétition 系 **18c.（TLFi 记 1759，Targe 译 Smollett《英国史》，引 Féraud）借自英语 competition**（Académie 9e 明记「emprunté de l'anglais」）；而英语 competition **c.1600 直承晚期拉丁 competitio、不经法语**（etymonline 直读；OED 1605 Bacon）；**Littré 系统旧说「← 拉丁 competitio」并列不折中**；运动义 late 19c./1931（J.-R. Bloch）；同根 compétent **ca 1240** → 英 competent late 14c. 借自法语 compétent（与库内英语页 [[competent]] **双向互链**）；英 compete 1610s 系逆构、competitive 1826
  - **équipe** ← 古法语 esquiper ← 古诺斯 **skipa**「装备船」← **skip「船」**（诺曼海事借层）；名词首证 **1456**（船组，罕见孤立）→ 1469（比武队，后废）→ 1864（工班）→ **1890 复兴**为「运动队」；动词 équiper **ca 1120 *eschiper***（《圣 Brendan》252）；英 **equip 1520s ← 法语 équiper**、équipage 1570s、equipment 1717、équipe 作法语借词保留；**ship / skipper 同出 ON skip / OE scip**——**équipe ↔ team 同义异源**（team ← OE tēam ← PIE \*deuk-「引导」，运动义 1846）；写法仿 [[vie]]↔life 对照
  - **quand** ← 拉丁 **quando**（← PIE **\*kʷo-**）；首证 **1050《圣 Alexis 传》**（Alexis 98，同时义；Alexis 150 因果义「既然」），另一说 Usito 记 **10c.**——并存；**勘正**：842《斯特拉斯堡誓言》的 *in quant* 系拉丁 **quantum**（in quantum），**非** quando——已在页内点明；英 **when** ← 古英语 hwænne ← **PIE \*kʷo-**，与 quand 系**共祖非借入**（quondam 系拉丁直借）；**易混三同音**：quand / quant（quant à）/ qu'en
  - **que penses-tu de**（多词短语页）：inbox `que pense-tu de` **缺 -s**——正确为 **Que penses-tu de… ?**（已勘正并标**待确认**）；三语域（倒装正式 ／ Qu'est-ce que tu penses de… ? 日常 ／ Tu penses quoi de… ? 极口语，quoi 原位）；并行 **Qu'en penses-tu ?** 互链 [[en-pron]] / [[en-parler]]；penser ← 拉丁 **pensare**（pendere「悬挂／称量」的反复态）——英 **pensive** late 14c. 借自法语 pensif；**penser ↔ think 同义异源**（think ← 日耳曼 \*thankjan）；`penser` 登待建
- **待回补（≈ 20 项）**：①首证并存两说（désert ca 1100/1080、quand 11c./10c.；équipe 名词唯得词典转写、TLFi 原条未直连）；②词源／造词诸说（comédie kōmos 三说、ciné Lumière/Bouly 与 1905/1908/1917、cinéma 1899/1900、désert desertum/desertus）；③英语侧年代零星（équipe 入英年代、comedian/comedienne、dessert 1393→1549、military *désert* = *déserteur* 义无书面首证、Idiomatic register 判断）；④**勘正／待确认**（`que pense-tu de`→`que penses-tu de`；triste 未得《Alexis》/《罗兰》确证，只到古法语 trist 10c.）——明细在各词条「来源」节
- 更新：[[french-vocabulary]]（153→161、待处理 +第十二批（含批内重复/手误判定）、词条列表 +8、待回补 +第十二批、命名表 +5 词「-en」预置与 dessert 近形备注、词族候选 **+7**（kōmōidia / kinēma / deserere / tristis / petere / skip 北日耳曼海事层 / pensare·pendere）并更新 2 条（\*kʷo- +quand、英语回借簇 +compétition））；`index.md`（Backlog +第十二批、待建词条 +[[penser]]、Stats：fr 153→161，总 185→193）；`wiki/vocab/en/competent.md`（+相关行 → [[competition]]，updated 2026-10-03）；已 git 提交

## [2026-10-04] ingest | vocab: 英语第三批 11 词 → 11 词条（dissonance / reverberate / supplant / uproot / resentful / grueling / unattainable / pricey / airtime / gutter / widow）

- 来源：`raw/vocab-inbox.md` 第 41–51 行（**均未标注出处**——推断续第二批「one prompt」语境，**待确认**）；去重：前 40 行均已编译；无重复、无近形冲突（11 个 basename 全库唯一，未撞 Windows 保留名）
- 新建 11 页（3 批子代理并行，主代理做记账与双向互链）
- 词源要点（英语侧 **etymonline 直读**为主，当场核实）：
  - **dissonance** ← 拉丁 dissonantia ← **sonare「响」**；early 15c. 经古法 **and** 直承中世纪拉丁（**两路径并存不折中**）；音乐义 1590s；**cognitive dissonance 1957**（etymonline 未具名 Festinger——标待验证）；同族 consonance / assonance / sonorous / sonata
  - **reverberate** ← 拉丁 reverberare「击回」（re- + **verberare「鞭打」** ← verber）；1570s（及物）／声音义 1590s；**勘正**：brief 原拟「intransitive 1620s」实为与名词 **reverberation「回声」义**混淆；另有旧动词 reverberen（early 15c.）——与 [[dissonance]] 同属「声音」词族但**两条拉丁路线**（sonare 响 ／ verberare 击）
  - **supplant** ← 拉丁 supplantare「绊倒」（sub- + planta「脚掌」）；c.1300 首见于 agent noun **supplanter**，**古法 suplanter 或直承拉丁**（etymonline 并列、古法形无年代）；「取代（事物）」义 1670s；planta 兼「脚掌」与「幼芽」两义——英 plant 出后者、supplant 出前者
  - **uproot** —— 英语本土 up + root（root ← 晚期 OE rōt / 古诺斯 rót ← PIE \*wrād-）；**etymonline：比喻义 1590s（隐含于 *uprooted*）、字面义 1690s**——与本批 brief 原拟**方向相反**，已从 etymonline 并把 brief 版并列登记（**子代理纠正主代理提示的一例**）
  - **resentful** ← resent + -ful（1650s）；resent ← 古法 resentir（re- + sentir）c.1600 旧义 → 1620s 今义「怨恨」（**词义窄化**：原义「感觉、感激」）；resentment 1610s ← 法 ressentiment 16c.；同族 consent / sentiment / dissension
  - **grueling** adj. **1852**（etymonline）← gruel（v.）「使精疲力竭」（late 18c. 俚语 get one's gruel）；gruel（n.）← 古法 gruel ← **法兰克 \*grut / PIE \*ghreu-「磨」**；**「喂稀粥以折磨」说判为民间词源**（etymonline gruel 条无此内容），三说并列
  - **unattainable** = un- + attain（c.1300 ← 古法 ataindre ← 俗拉丁 \*attangere ← 拉丁 attingere = ad- + **tangere「触」**）+ -able（attainable 1640s）；**无独立 etymonline 条目**；同族 tangent / tangible / contact / tact
  - **pricey** **1932**（etymonline；更早 high-pricey 1923）← price + -y；price ← 古法 pris ← 拉丁 **pretium**；**pretium 一源多路**——英 price/precious/prize/praise/appreciate ／ 法 [[precieux]]/prix/apprécier（**双向互链已补**）
  - **airtime** —— 英语复合词 air（「广播」义 20c.）+ time；**etymonline 无词条**（已检索确认）→ 首现年代推断 1920s–30s，**标待验证**
  - **gutter** ← **古法 goutiere（Anglo-French gotere）** ← 古法 goute ← 拉丁 **gutta「一滴」**；13c. 末首见；檐槽 mid-14c.／冲刷沟 1580s／喻义「社会底层」1818／**印刷义（订口）1841**；同族 **gout**（c.1200，共祖）、guttersnipe 1857；法 **gouttière** 同源借入
  - **widow** ← 古英 widewe ← 原始日耳曼 \*widuwō ← **PIE 形容词 \*widhewo-**（词根 \*uidh-「分开」）；动词 early 14c.／**印刷义「寡行」1904**；**etymonline 未给名词首现年代**（须另觅 OED）；**法 veuve ← 拉丁 vidua ← 同一 PIE \*widhewo-——共祖非借入**
- **英法交叉（本批跨轨亮点）**：`gutter ↔ 法 gouttière`（**借入**）＋ `gout`（**共祖**）——same Latin **gutta**；`widow ↔ 法 veuve`（**共祖非借入**，日耳曼支 vs 拉丁支）；`uproot ↔ 法 déraciner`（**同训异源**；真共祖是 root ↔ racine，PIE \*wrād-）；`dissonance ↔ 法 [[son]]`（sonus 同族，**已补双向互链**）；`pricey ↔ 法 [[precieux]]`（pretium 一源两路，**已补双向互链**）；`resentful ↔ 法 ressentir / ressentiment`（借入）
- **待回补（≈ 36 项）**：三类（①airtime / unattainable / grueling 无独立 etymonline 条目 ②诸说并列不折中 ③未核细节）——见 index 与各页「来源」节
- 更新：[[english-vocabulary]]（32→43、待处理 +第三批、词条列表 +11、待回补 +第三批、词族候选 **+4**（声音两路线 / dis- 簇 / \*widhewo- 同义异源 / pretium 两面）并更新印刷簇（**+gutter 印刷义 1841、+widow 印刷义 1904**））；`index.md`（Backlog +第三批、Stats：en 32→43，总 193→224）；`wiki/vocab/fr/courageux.md`、`precieux.md`、`son.md`（**补双向互链**、updated 2026-10-04）

## [2026-10-04] ingest | vocab-fr: 第十三批 20 条 → 20 词条（quel / comment / pourquoi / chaque / souvent / expliquer / recommander / regarder / couter / ecrire / erreur / titre / fois / dernier / jeune-adj / parce-que / comme-toi / aide-moi / d-accord / nul-adj）——含 5 个多词短语页与 2 处保留名/近形消歧

- 来源：`raw/vocab-fr-inbox.md` 第 179–198 行（均**未标注出处**——推断 Duolingo，**待确认**）；去重：前 178 行均已编译；批内无重复
- 新建 20 页（4 批子代理并行，主代理做记账与双向互链）：疑问/副词组 [[quel]]、[[comment]]、[[pourquoi]]、[[chaque]]、[[souvent]]；动词组 [[expliquer]]、[[recommander]]、[[regarder]]、[[couter]]、[[ecrire]]；名词/形容词组 [[erreur]]、[[titre]]、[[fois]]、[[dernier]]、[[jeune-adj]]；短语组 [[parce-que]]、[[comme-toi]]、[[aide-moi]]、[[d-accord]]、[[nul-adj]]
- **命名消歧（2 处，已入 hub 登记表）**：①`nul` 系 **Windows 保留名**（§3.1）→ `nul-adj.md`（`word` 保留 `nul`）；②`jeune`（adj.）与 `jeûne`（n.m. 禁食）ASCII 转写同名 → 按「同语言近形一律先用词性后缀」预置 `jeune-adj.md`（同 [[sur-prep]] 例），日后 jeûne 记 `jeune-nm.md`
- **lemma 判定待用户确认（2 处）**：`coute`→[[couter]]（coûter；同形亦可读 `couteau` 截断）、`ecrit`→[[ecrire]]（亦可读名词 `l'écrit`）；页内均加「收录说明」
- 词源要点（法语侧经检索转引 CNRTL/TLFi、DHLF、Académie 9e；**CNRTL 直连仍全部 sinkhole**——本批 5 次直取均只回「Portail lexical」壳；英语侧 etymonline 直读）：
  - 疑问词族：**quel** ← 拉丁 qualis（← \*kʷo- + -alis）；首证 10c 下半叶（圣 Léger 149）／ca 1100 罗兰；**勘正**：英 such 与 qualis **非亲缘**（← \*swalikaz），which 系共祖；**comment** ← 拉丁 **quomodo**（＝ comme 同源）；首证 ca 1100 间接疑问（罗兰 3872）／1080（DHLF 另说）；**pourquoi** ← pour（← pro）+ quoi（← quid）；首证 **ca 1050 pur quei**（圣 Alexis 131/444）；**chaque** ← 拉丁 **quisque** → 通俗 \*cascu；chaque 无复数、与代词 chacun 分工
  - **souvent** ← 拉丁 **subinde**「随即、屡屡」（sub + inde；inde 又出法语 en）→ 古法 sovent（**ca 1050 圣 Alexis，TLFi 首证**）→ 13c réfection；**拉丁 saepe 消失后由 subinde 接替其「经常」义**（Löfstedt，TLFi 引）；意 sovente **借自**古法；英 often ← 日耳曼 \*ufta-（**同义异源**，深度来源不明）
  - 动词组：**expliquer** ← 拉丁 **explicare**（ex- + plicare）14c 学界借入（首证 1362-65）；**勘明英 explain ← explanare 系另一词根分支，英 explicate/explicit 才与 expliquer 同源**；**recommander** ← re- + commander ← commendare（command/commend/recommend **同一词根三次入英**）；**regarder** ← **法兰克 \*wardōn**（日耳曼底层非拉丁），首证「注视」义 1080；英 ward（共祖）/guard（借入）/regard（借入）——w-/gu- 分化 doublets；**coûter** ← 拉丁 constare → 俗 \*costare（12e s.）；英 cost 同源借入；**écrire** ← 拉丁 scribere（首证 ca 1050 Alexis 284 **escrit**）；英 write ← 日耳曼 **同义异源**
  - 名词/形容词组：**erreur** ← 拉丁 error（首证 fin xe s. *error*「欺骗」，现代义 ca 1200）——**TLFi 该条只列 error、未提 errare**（与 etymonline 并列）；**titre** ← 拉丁 titulus（古法 title ca 1170；**法异化去 -l-**，英 title 反保留 -l-）；**fois** ← 拉丁 vicis 的复数 vices → 古法 feiz；v- → f- 两说；**单复数同形**；英 vicissitude **共祖但承「更迭」义**；**dernier** ← 通俗拉丁 **\*deretranus**（de retro + -anus）→ derrain → -ier 类比 premier → dernier——**三形并列**（\*deretranus / \*deretranacius；brief 原拟 `deretinus` 诸源查无已剔除）；**jeune** ← 通俗 \*jovenis ← juvenis（PIE \*yeu-）；英 juvenile/junior 拉丁支 **vs** young 日耳曼支——**同根不同支**
  - 短语组：**parce que** = par + ce + que（古法 por ço que；Académie 记 12c）；**勘明英 because 系「仿法语 *par cause*」的平行构式、非借入**（且 etymonline 对照的是 par cause 非 par ce que——**待验证**）；**comme toi**：comme ← quomodo（与 [[comment]] 同源）＋ **重读代词 toi**（非 tu）；**aide-moi** ← aider ← adjutare；命令式肯定式**代词后置连字符**；英 aid 借入（← 古法 aide）vs adjutant 拉丁直借；**d'accord** ← ad + **cor「心」**（PIE \*kerd-）→ 古法 acord；英 accord 借入；**已与 [[courageux]] 双向互链**；**nul** ← 拉丁 nullus（ne- + ullus）；首证 **842《斯特拉斯堡誓词》**；**英 null 经 Anglo-French nul 借入**（**非**直承拉丁——**子代理勘正 brief**）；nul 的「纹章学义」检索无据**已剔除**
  - **易混辨析页内点明**：英 comment（← 拉丁 commentum ← \*men-）与法语 [[comment]]（← quomodo）**同形而词源全异**；fois / foie / foi 同音
- **待回补（≈ 75 项）**：四类（①首证并存两说 ②词源重构诸说 ③lemma 判定待确认 ④**chaque 全条风险最高**、recommander 首证未取得）——见 index 与各词条「来源」节
- **待建信号（已入 index Backlog）**：[[comme]]（优先——[[comment]] / [[comme-toi]] 反复需引）、`jeûne`→`jeune-nm.md`、`couteau`
- 更新：[[french-vocabulary]]（161→181、待处理 +第十三批、词条列表 +20、待回补 +第十三批、**命名表 +2 行（nul-adj / jeune-adj）**、词族候选 **+11**（quisque·qualis / vicis / titulus / errare / scribere / subinde / retro / juvenis 三支 / adjutare·juvare / parce que·because 平行构式 / gutta 与 \*widhewo- 跨轨两条）并更新 2 条（PIE \*kʷo- **+quel/comment/pourquoi＝已收 6 词、可评估建页**；cœur/cor +d-accord）、开放问题 +CNRTL sinkhole 复核 +待建信号）；`index.md`（Backlog +第十三批、待建 +[[comme]]、Stats：fr 161→181，总 193→224）；`wiki/vocab/fr/courageux.md`（⇄d-accord 互链）、`son.md`（+dissonance 互链）

## [2026-10-04] ingest | vocab + vocab-fr 合并提交（上文两条同一次会话；31 页新词条 + 3 页补链）——待用户确认项清单

- 本日一次会话内完成**两语轨** ingest（用户「ingest this」→ 确认「两条都 ingest」）：英语第三批 11 词 + 法语第十三批 20 条 = **31 页新词条**
- **需用户确认（4 项，为 CLAUDE.md §3.1 第 1 步与法语细则要求）**：
  1. `coute` → 判为 **coûter**（coûte）——同形亦可读 **couteau** 截断；若为 couteau，请告知，我改收 couteau 页
  2. `ecrit` → 判为**动词 écrire** 的过去分词/现在时——亦可能为名词 **l'écrit**（文字作品）
  3. `jeune` → 按近形规则建 **`jeune-adj.md`**（与 jeûne 撞名预置）——确认无误则日后 jeûne 记 `jeune-nm.md`
  4. **第三批英语词出处**：inbox 第 41–51 行**未标注来源**（第二批标「From one prompt」）——是否同源？
- git：本次 `ingest: vocab 第三批 11 词 + vocab-fr 第十三批 20 条`

## [2026-10-04] ingest | vocab: 英语第四批 19 词 → 19 词条（sturdy / vigilant / hostility / vehement / tribe / heme / mince / amino-acid / lentil / endotoxin / limestone / hard-to-abate / micronutrient / fortify / fermentation / tap-into / crackle / deliberate / premise）——含本库首例**跨页矛盾显式并存**

- 来源：`raw/vocab-inbox.md` 第 52–70 行（**均未标注出处**——推断延续前批语境，**待确认**）；去重：前 51 行均已编译；19 个 basename 全库唯一（未撞 Windows 保留名；`amino-acid` / `hard-to-abate` 为多词、`tap-into` 为短语动词，均按 v1.5 约定命名）
- 新建 19 页（4 批子代理并行，主代理做记账）
- 题材：**营养／可持续 + 生物医学 + 通用抽象**；**古法借入层密集**（本批 8 词英 ← 古法）
- 词源要点（英语侧 **etymonline 直读**为主，8–9 次/组）：
  - **sturdy** ← 古法 **estourdi / estourdir**「使眩晕」← 俗拉丁 \*exturdire ← 拉丁 ex- + **turdus「鸫鸟」**（**「鸫鸟吃葡萄吃到醉」**的著名理据——etymonline 明标 *the usual suggestion*，**OED 判其旁证「严重可议」**，Century Dictionary 另拟拉丁 torpidus——**三说并列不折中**）；c.1300 首义「鲁莽」→ late 14c. 转「结实」（**义变是重点**；法语同源词 étourdi 另走「心不在焉」）
  - **vigilant** ← 拉丁 vigilare「守夜」← vigil「醒着的」← vigere；late 15c.；法 **veiller / veille / réveiller / surveiller** 一族同出 vigilare——**英语只借走形容词，法语把动词支自己发展**
  - **hostility** ← 拉丁 hostilitas ← hostilis ← **hostis「敌人」（原义「陌生人」）**；early 15c.；**hostis 与 hospes 同源**——「客与敌同根」（与 [[hospice]] 互链）
  - **vehement** ← 拉丁 vehemens——**今说 vehere「载」**（PIE \*wegh-，与 vehicle 同族）vs OED 旧说 vehe-+mens（de Vaan 2008 辨正）**两说并列**；early 15c. 经古法
  - **tribe** ← 拉丁 **tribus**「古罗马三政治／族群分区」——**etymonline 直言 unknown origin**（tri-「三」说记 generally held）；mid-13c. 首指以色列十二支派 → 1590s 泛化
  - **heme** ← 希腊 **haima「血」**（属格 haimatos）→ 德语 Häm → heme——**etymonline 无 heme 词条**，「haima→Häm→heme」中间环节与造词者**均待验证**；同族 hemoglobin（1862 ← hæmatoglobin 1845）、-emia；**与 [[endometriosis]] 的希腊零件族互见**
  - **mince** ← 古法 **mincier** ← 俗拉丁 \*minutiare ← 拉丁 **minutus**「小」；late 14c.；**同根 `minute` / `menu` / `minutiae` / `diminish`**；法今用 hacher / émincer
  - **amino-acid**：amino- ← amine **1863**（= ammonia + -ine）+ acid（← acidus ← acere）；amino- 构词 **1887**、amino acid **1898**；**勘正 brief**：「amino- 1838」与 etymonline 不符
  - **lentil** ← 拉丁 **lens / lentis**「扁豆」→ 古法 lentille（← 拉丁指小词 lenticula）；英语 **mid-13c.**（inbox 作 c.1200——**两说并列**）；**lens（透镜，1690s 直借）与 lentil 同根**——同一拉丁词两条英语来路
  - **endotoxin** ← 希腊 endon「在内」+ **toxikon「箭毒」**（← toxon「弓」；toxon 有斯基泰借词说 vs Beekes 前希腊语——**两说**）+ -in；**造词史两说并列**：Pfeiffer 1892 观察／1904 印行 **vs 首印 1903**（Wolff 演讲、Rehns 综述）；**与 [[endometriosis]] 共享 endo- 零件**
  - **limestone** = lime（OE līm「黏物」← PIE \*(s)lei-）+ stone；本土复合词 **late 14c.**，得名「烧之出石灰」；**三 lime 同形**（矿物／酸橙 **1630s**／椴树 1620s）——**acid 橙路径勘正**（西 lima / 葡 limão / 阿 lima，brief 的「法 ← līmah」etymonline 不支持）
  - **hard-to-abate** —— hard + to + abate（**c.1300** ← 古法 abatre ← 俗拉丁 \*abbatere ← ad + battuere「打」）的复合形容词；**etymonline 无词条**（直取返回首页）——2010s 气候／能源话语新词，**首见年代待验证、未臆造**
  - **micronutrient** —— micro-（希腊 mikros）+ nutrient（← nutrire）；**etymonline 无词条**——20c 营养学造词，年代待验证
  - **fortify** ← 古法 **fortifier** ← 晚期拉丁 **fortificare**（fortis + facere）；英 early 15c.（**医学义「增效」为先**）→ mid-15c 军事 → late 15c 道德 → 1880「掺酒」→ **1939「添加营养素」**——**双义并存**（军事＋营养）
  - **fermentation** ← 晚期拉丁 fermentatio ← 拉丁 **fermentum**「酵母」（可能缩合自 \*fervimentum ← fervere「沸」← PIE \*bhreu-）；英语 **late 14c.**（炼金术）→ 科学义 c.1600 → 比喻义 1650s；法 fermentation 同形同源
  - **tap-into** ← OE **tæppian**「给…装龙头」← tæppa「龙头」；**勘正 brief**：OE 义为「装龙头」而非「开桶取酒」（后者 c.1400）；引申义「打开以供取用」1570s、窃听 1869；**两个 tap 同形异源**（取液 ← OE tæppian／轻敲 ← 古法 taper）
  - **crackle** ← mid-15c. crackelen = **cracken 的反复态**（OE cracian ← PGmc \*krakojan；德 krachen 同源）+ -le；名词「噼啪声」1833（inbox 作 late 14c.——**两说并列**）
  - **deliberate** ← 拉丁 **deliberare**「仔细权衡」（de- + **librare「称量」← libra「天平」**）；adj. early 15c.／现代动词形 1540s，**但最早动词记录 deliberen（late 14c.）反早于形容词**——brief 的「adj. 先于 v.」须加此限定；**libra「天平」 vs liber「书」（library）两线分清**
  - **premise** ← 古法 premisse ← 中世纪拉丁 **praemissa**（propositio praemissa）← praemittere（prae- + mittere）；逻辑义 late 14c.；**`premises`「房舍」义与逻辑义同源分化**（← 法律文书里先列的「上述不动产」）→ 1730——义变重点
- **英法交叉（本批几乎全是英 ← 古法借入）**：`sturdy ↔ 法 étourdi`（借入，**借后义走相反方向**）；`mince ↔ mincier`（借入）；`fortify ↔ fortifier`（借入）；`vigilant ↔ vigilant`（借入）；`hostility ↔ hostilité`（借入）；`vehement ↔ véhément`（借入）；`tribe ↔ tribu`（借入）；`premise ↔ prémisse`（同借中世纪拉丁）；`deliberate ↔ délibérer`（共祖＋英早期动词经古法）；`lentil ↔ lentille`（**共祖**，拉丁 lens 一词两语分工相反）；`fermentation ↔ fermentation`（同形同源）；`endotoxin ↔ endotoxine` / `heme ↔ hème` / `amino acid ↔ acide aminé`（同构科学词）；`tap into ↔ exploiter / puiser dans`（同训异源）；`crackle ↔ crépiter`（**平行拟声**，同一反复态构词路）；`limestone ↔ calcaire / chaux`（同训异源；英 chalk 与法 chaux 同出拉丁 calx）；库内已互链：[[micronutrient]] → [[micro-ondes]]、[[fermentation]] → [[cuisine]] / [[recette]] / [[produit]] / [[culture]] / [[tarte]]、[[hostility]] → [[hospice]]
- **跨页矛盾（本库首例，按 §5 显式并存、不折中）**：代理指出 etymonline 的 PIE 根条目**只记 \*ghos-ti-**（"stranger, guest, host"，并列为同族 hospes / hostis / guest / xeno-），**未记 \*ghos-pot-**——而 [[hospice]]（2026-10-01 建，原 `updated` 2026-10-01）页记的是 **\*ghos-pot-**「陌生人之主」（＝ \*ghos-ti- + \*poti-「主人」的复合构拟）。**处置（由主代理直接执行，未静默覆盖）**：①在 [[hospice]] 页「词源/构词」新增「**并存记录（2026-10-04，不折中）**」小节，写明两说来源与差异；②hospice 页补 [[hostility]] 互链、`updated` 改 2026-10-04；③**待回补**核实 \*ghos-pot- 构拟的权威出处（de Vaan 等）
- **待回补（≈ 58 项）**：四类（①无权威条目 ②首见两说 ③造词史并列 ④已勘正 brief 五处）——见 index 与各页「来源」节
- **连带待建（法语轨，已入 [[french-vocabulary]] 待建信号）**：本批 19 词牵出约 30 个法语对应词（étourdi / vigilant 家族 / hostilité / hôte / véhément / tribu / hème / hacher / émincer / lentille / endotoxine / fermentation / calcaire / chaux / abattre / abattoir / fortifier / micronutriment / oligo-élément / prémisse / délibérer / exploiter / puiser / robinet / crépiter / craquer / acide aminé 等）——**其中 5 个与英语页 ASCII 撞名**（vigilant / véhément→`vehement` / mince / hème→`heme` / fermentation），**命名表已 +1 行预置 `-fr` 后缀**
- 更新：[[english-vocabulary]]（43→62、待处理 +第四批、词条列表 +19、待回补 +第四批 +跨页矛盾条、**词族候选 +5**（\*ghos-ti- 客与敌同根 / 古法借词层样本组 / lens 两面 / 营养与食品科学簇 / libra vs liber）并更新 1 条（希腊医学构词零件 +heme/endotoxin））；[[french-vocabulary]]（命名表 **+1 行**（5 个 `-fr` 预置）、开放问题 +第四批连带待建信号）；`index.md`（Backlog +第四批、Stats：en 43→62，总 224→243）；`wiki/vocab/en/hospice.md`（**新增并存记录小节 + [[hostility]] 互链**、updated 2026-10-04）
- **需用户确认（1 项）**：英语第三、四批（第 41–70 行）**均未标注出处**——是否同源（第二批标「From one prompt」）？

## [2026-10-04] query | 词头总表：en 62 + fr 181 = 243 → 产物 output/2026-10-04-vocab-headwords.md

- 用户问「能否总结所有英法词头、有没有命令」——**本库无现成命令**（词条不逐条进 index，只能靠 hub 词条行 / 搜索 / graph）；本次以脚本从各页 frontmatter `word` 字段提取（只读，不写 `raw/`）
- 产物落盘 `output/2026-10-04-vocab-headwords.md`：①统计（en 62 ｜ fr 181 ｜ 合计 243；learning 全 new）②英语 62 按字母（标出 7 个多词条目）③法语 181 按字母（点明 `la` 双页：[[la-art]] / [[la-pron]]；`en` 指 [[en-pron]]）④附录「文件名 ≠ 词形」三类对照表（重音转写 44 条 / 多词单位 11 条 / 词性后缀与保留名 5 条）⑤生成脚本（PowerShell 只读，可重跑）
- **一致性核对**：词头总数与两 hub 计数及 index Stats 完全一致（en 62 ｜ fr 181 ｜ 243）——顺带验证了 hub 记账无误
- 更新：`index.md`（Outputs 区 +1、Stats Outputs 1→2）；log 本条
- 待办（用户选择）：是否加**一条斜杠命令**（`.claude/commands/`）或**装 Dataview** 以便随时刷新/实时查看——见对话中给出的两个方案

## [2026-10-04] schema | 新增斜杠命令 /headwords（`.claude/commands/headwords.md`）

- 用户确认选择「加斜杠命令」（备选 Dataview 插件保留为未启用方案）
- 新建 `.claude/commands/headwords.md`：刷新词头总表产物 `output/YYYY-MM-DD-vocab-headwords.md`——步骤含 ①脚本提取 frontmatter `word` ②**核对两 hub 与 index 计数、不一致先报告不静默改数**（§5 第 5 条）③落盘（快照式覆盖，含「文件名 ≠ 词形」三类对照）④index Outputs + Stats、log 记账 ⑤回报；参数 `en` / `fr` 可只出单语
- 同步改 `CLAUDE.md`：§6 环境与工具 +1 条（斜杠命令；并把 Dataview 备注改为「装上后亦可用它实时列词头」）、§7 演进记录 +1 条（2026-10-04）
- 说明：`.claude/` 此前不存在，且未被 `.gitignore` 排除 → 命令文件入库版本管理
- 备注：本次连同上一条 query 条目一并提交

## [2026-10-04] query | 词头总表：en 62 + fr 181 = 243 → 产物 output/2026-10-04-vocab-headwords.md

- **`/headwords` 命令首次运行**（命令建立于同日 schema 条目）：脚本从 `wiki/vocab/en|fr` 各页 frontmatter 提取 `word` / `learning`（只读，未动 `raw/`）
- 产物 `output/2026-10-04-vocab-headwords.md`：快照式覆盖——内容与上一版**逐条比对无差异**（en 62 / fr 181 两清单 Compare-Object 均空），仅改写「刷新记录」尾注（记本次刷新 + inbox 待编译提示）
- **一致性核对**：live 提取 en 62 ｜ fr 181 ｜ 合计 243，与 [[english-vocabulary]]（词条 **62**）、[[french-vocabulary]]（词条 **181**）及 `index.md` Stats 行**三处完全一致**，无差异可报；`learning` 全为 new（243/243）
- 本次**无新词条并入**（自 2ae8684 以来 vocab 目录未变）；`index.md` Outputs 行与 Stats 行数值未变，故未改动
- 待办：`raw/vocab-inbox.md` 有 **22 词待 ingest**（contaminant / paygrade / microorganism / graze on / legume / deluded / habitat / thermostat / coil / stifle / thermoregulate / hefty / retrofit / furnace / rebate / insulate / indigenous / evaporate / brine / microbial / phosphate / unearth）——编译后本表将变为 en 84 / 合计 265

## [2026-10-04] ingest | vocab: 第五批 22 词（环境微生物 / 土地农业 / 热工 / 建筑节能 / 通用）→ 22 新页

- **来源**：`raw/vocab-inbox.md` 第 **71–92 行** 22 条（**均未标注出处**——推断延续前批语境，**待用户确认**，同第二至四批情形）；`raw/` 未作任何修改
- **方式**：5 个子代理**并行编译**（环境·微生物 4 · 土地农业 4 · 热工 4 · 建筑节能工业 5 · 通用 5），一律 **etymonline 直读**为权威源；派生词无独立条目者回落基础词条并**在页内注明交叉路径**（如 contaminant→/word/contaminate、thermoregulate→/word/thermo- + /word/regulate、paygrade→/word/pay + /word/grade）
- **建页 22**（`wiki/vocab/en/`，均 growing / learning: new）：[[contaminant]]、[[paygrade]]、[[microorganism]]、[[graze-on]]、[[legume]]、[[delude]]、[[habitat]]、[[thermostat]]、[[coil]]、[[stifle]]、[[thermoregulate]]、[[hefty]]、[[retrofit]]、[[furnace]]、[[rebate]]、[[insulate]]、[[indigenous]]、[[evaporate]]、[[brine]]、[[microbial]]、[[phosphate]]、[[unearth]]
- **lemma / 命名判定**：①`deluded`（inbox 第 76 行）按 schema「lemma 取词典 headword」**归并为动词 [[delude]]**——页面顶部加「收录说明」，分词 / 形容词用法一并讲；**lemma 判定待用户确认**；②`graze on` 为**多词 / 短语动词** → `graze-on.md`，frontmatter `word: "graze on"`；③与既有 243 页**无重名、无近形撞车**（建页前已全库扫描）
- **词源收获（本批最值得记的一条）**：**[[furnace]]（拉丁 fornax / formus）与 [[thermostat]] / [[thermoregulate]] / thermal / thermometer（希腊 thermos / thermē）同出 PIE \*gwher-「加热」——共祖而非借入**，etymonline 在 furnace 与 thermo- 两条下**各自独立**指向 \*gwher-，四页已互链成一条**经双源佐证的同根链**
- **诸说并列 / 未定论（一律不折中）**：microorganism 首见 **1855（etymonline）/ 1880（OED，MacCormac）/ 1875–80（Collins）** 三说；retrofit **1954（USAF）vs OED 引文 1951**（另 1949 一说）；coil 名词 **1620s（etymonline，由动词来）vs 1567（MW）/ 1560–70（TFD，标 origin uncertain、或 cull 变体）**；insulate 物理义 **1742 vs 约 1741（MW）**；thermoregulation **1920s（OED 引例 1927）vs 法语 1904**；thermostat 造词年 **1831 vs 1830**（均系 Andrew Ure）；stifle 名词首见书各记不一（1275–1325 / 1350–1400）；**brine 与 vapor 经 etymonline 明记 of unknown origin（brine 且未给年代）**、warm「不明」而 \*gwher- 仅两猜之一、legume 拉丁 legumen unknown origin
- **已勘正的 brief 偏差**：`graze on` 的「擦伤义与吃草义**同形异源**」一说被 etymonline 的 **「possibly a transferred sense of graze (v.1)」（倾向同源）** 取代，页内两说并列；`coil` brief 所给年代经 etymonline **完全印证**（v. 1610s ← 古法 coillir ← 拉丁 colligere；n. 1620s 航海 → 1660s 环状 → 1849 电学）
- **交叉引用**：新建页互链（组内）＋ **回补旧页入链 8 处**——[[lentil]] → [[legume]]、[[reverberate]] → [[furnace]]、[[fermentation]] → [[microorganism]] / [[brine]]、[[sterile]] → [[microorganism]] / [[microbial]]、[[stifle]] → [[stiff]]、[[unearth]] → [[unravel]]、[[habitat]] ↔ [[indigenous]]（双向）
- **更新**：[[english-vocabulary]]（词条 62→**84**、待处理 +第五批 22 条、词条列表 +22、**待回补 +第五批（≈73 项）**、**词族候选 +7**：\*gwher- 共祖链 / 建筑节能「手段—动作—激励」链 / 微生物与污染簇 / 拉丁 insula 一源多分 / ludere 族 / 豆科与放牧簇 / 盐的两条线）；[[french-vocabulary]]（**命名消歧登记表 +1 行**（legume · habitat · thermostat · phosphate 四词预置 `-fr`）、**词族候选 +3 跨轨**（insula · \*gwher- · légume faux ami）、**待建信号 +第五批连带**法语对应词清单）；`index.md`（Stats：en 62→84、总 243→**265**；Backlog +第五批待回补 + 第五批连带待建清单）
- **待办**：①**需用户确认**：第五批 22 条**均未标注出处**（第 71–92 行）是否延续前批语境；`deluded → delude` 的 lemma 判定；②待回补 ≈73 项（明细见各页「来源」节与 index/hub）；③法语轨第五批连带收词清单已入 [[french-vocabulary]]（含 **étouffer**——正是 stifle 动词可能源头古法 estouffer 的现代形）

## [2026-10-05] ingest | vocab: 第六批 45 词（物件装饰 / 身体医学 / 心理态度 / 劳作工艺 / 社会抽象）→ 45 新页

- **来源**：`raw/vocab-inbox.md` 第 **93–137 行** 45 条（**均未标注出处**——推断延续前批语境，**待用户确认**，同第二至五批情形）；`raw/` 未作任何修改
- **用户裁定 3 处笔误 / 听记偏差**（AskUserQuestion，2026-10-05）：`shringle`（第 98 行，查无此词）→ **shingle**；`scarve`（第 132 行）→ **scarf**；`spread oneself with`（第 94 行）→ **spread oneself thin**——三页「收录说明」均已记录 inbox 原词形
- **lemma 归并 3 处**：`intimidating`→[[intimidate]]、`decompressing`→[[decompress]]、`crippling`→[[cripple]]（各页「收录说明」记录原词形）
- **方式**：5 个子代理**并行编译**（物件装饰 · 身体医学 · 心理态度 · 劳作工艺 · 社会抽象，各 9 词），一律 **etymonline 直读**为权威源；短语动词 / 习语无专条者回落成分词并**在页内注明交叉路径**
- **建页 45**（`wiki/vocab/en/`，均 growing / learning: new）：[[shingle]]、[[bevel]]、[[brass]]、[[chandelier]]、[[purse]]、[[adorn]]、[[scarf]]、[[dumpster]]、[[firepit]]、[[tendonitis]]、[[cortisol]]、[[plaque]]、[[cripple]]、[[decompress]]、[[tenuous]]、[[subfreezing]]、[[linger]]、[[dry-up]]、[[intimidate]]、[[disdain]]、[[mischievous]]、[[resentment]]、[[unrelenting]]、[[undeterred]]、[[slacker]]、[[grounded]]、[[catch-off-guard]]、[[juggle]]、[[intricate]]、[[refurbish]]、[[haul]]、[[tailgate]]、[[scoot]]、[[carve-out]]、[[push-through]]、[[cling-to]]、[[jack-of-all-trades]]、[[spread-oneself-thin]]、[[stench]]、[[memoir]]、[[counterbalance]]、[[lifeline]]、[[recession]]、[[relinquish]]、[[shrine]]
- **多词 / 短语动词 7 条**：习语 [[jack-of-all-trades]] · [[spread-oneself-thin]] · [[catch-off-guard]] ＋ 短语动词 [[dry-up]] · [[carve-out]] · [[push-through]] · [[cling-to]]
- **词源收获（本批最值得记的一条）**：**[[shrine]] 的 \*sker- 假说被否**——原拟「与 scribe / script 同出 PIE \*sker-『切、刻』＋ 文书匣→圣物匣→圣地三阶段演变」，但 etymonline 于 scrinium 条**明注 "a word of unknown origin"**、既无 \*sker- 亦无三阶段表述——页内**不采信 \*sker-**，仅把「文书匣 → 圣物匣 → 圣地」作**语义链推演**（据古英语 scrin 义 + 中古英语 shrine 义并置）；另一条：**[[counterbalance]]（1610s）早于 [[counterweight]]（1690s 前后）**——「抽象制衡」先于「具象配重物」，与直觉相反（两页已互链辨析）
- **诸说并列 / 未定论（一律不折中）**：`scarf` n.1「围巾」三说并存（或 ← 古北法语 escarpe）；`stench` 由 **OE stenc「气味」（香臭皆可的中性词）→ c.1200 起 predominated 专指恶臭**（语义降格标本，转折确切年代待回补）；`mischievous` 由 early 14c.「不幸的」→ 1670s「淘气」；`slacker` 1897 首见但**「逃避兵役者」义 etymonline 未提**（标待验证）；`grounded` 人格义「脚踏实地」首见未单列；**词源不明**：brass（英语外无同源词）· scoot（1758，uncertain）· bevel（或 ← 古法 \*baivel）· shingle n.2 · plaque（佛兰芒中介 possibly）· relent（unrelenting 底词）· timere（intimidate 底词）· scrinium
- **已勘正的 brief 偏差**：①**shrine 的 \*sker- 不采信**（见上）；②**tailgate**：提示「1880s 尾随义」与 etymonline 不符，已按 **1951** 记录（n. 1868 货车尾板 / by 1950 汽车尾门 / by 1958 tailgate party）；③**disdain**：etymonline 作 des- + deignier ← **dignari**（非提示的 de- + digne）；④**slacker**：古英语拼作 **slæc**（非 sleac），且 etymonline 明记 slacker 与古英语名词 sleacornes「懒惰」**不同源**；⑤**resentment**：**etymonline 记 resentful 为 1650s**（非提示的约 1600），故 **resentment（1610s）早于 resentful（1650s）**——已与既有 [[resentful]] 页互链对齐
- **交叉引用**：新建页组内互链 ＋ **回补既有页**——[[counterweight]]（**新增 [[counterbalance]] 互链 + updated 2026-10-05**，见物件装饰组的越界允许范围内改动）、[[resentful]] ↔ [[resentment]]（双向）、[[endometriosis]] ↔ [[tendonitis]]、[[endotoxin]] ↔ [[cortisol]]、[[decor]] / [[orderly]] ↔ [[adorn]]、[[gutter]] ↔ [[dumpster]]、[[furnace]] ↔ [[firepit]] / [[subfreezing]]、[[evaporate]] ↔ [[dry-up]]、[[thermostat]] / [[insulate]] ↔ [[subfreezing]]、[[menacing]] / [[hostility]] / [[vehement]] ↔ 本批心理簇、[[unattainable]] ↔ [[unrelenting]] / [[undeterred]]
- **格式自检（脚本，45/45 通过）**：frontmatter 七项字段齐全（type / lang / sources / status / learning / created / updated / word / ipa / pos）、每页**恰一个** `??` SRS 分隔符、闪卡小节均带 `#flashcards/en`、正文无游离 `?`（唯一例外 [[unrelenting]] 正文含**全角 `？`**，为中文标点、非分隔符）
- **悬空链接核对（全库）**：本批**新增 0 条悬空 wikilink**——现存 4 条（comte / grand / lentille / vivre）**均为法语侧旧有待建信号**、已在 [[french-vocabulary]] / index Backlog 登记
- **更新**：[[english-vocabulary]]（词条 84→**129**、待处理 +第六批 45 条、词条列表 +45、**待回补 +第六批（≈40 项）**、**词族候选 +7**（un-+分词簇 / 拉丁 caput 一源多分 / 拉丁 linquere 族 / 拉丁两条「怕」timere vs terrere / balance 家族 / PIE \*ten- 族 / 名物词语义漂移标本组 / sentire 族补齐））；`index.md`（header 最近更新 2026-10-05；Stats：en 84→**129**、总 265→**310**；Backlog +英语第六批待回补）；log 本条
- **待办**：①**需用户确认**：第六批 45 条**均未标注出处**（第 93–137 行）是否延续前批语境；②待回补 ≈40 项（明细见各页「来源」节与 index/hub）；③法语轨：本批英语词牵出的法语对应词（cortisol / plaque / tendinite / dédain / méchant / ressentiment 等）尚未清点入 [[french-vocabulary]] 连带待建信号

## [2026-10-05] ingest | vocab-fr: 第十四批 52 条 → 47 新页 + 5 处去重补充（家居名词 / 抽象名词 / 动词 / 形容词 / 虚词代词）

- **来源**：`raw/vocab-fr-inbox.md` 第 **199–250 行** 52 条（**均未标注出处**——推断 Duolingo，**待确认**，同第三至十三批情形）；`raw/` 未作任何修改
- **方式**：6 个子代理**并行编译**（家居日常名词 10 · 抽象名词 7 · 动词 11 · 形容词 7 · 虚词副词 7 · 代词 5），一律 **etymonline 直读**（英语侧）＋ **fr.wiktionary（引 TLFi/DMF）直取 + WebSearch 转引 CNRTL/TLFi**（法语侧——**CNRTL 直连仍全部 sinkhole**）
- **建页 47**（`wiki/vocab/fr/`，均 growing / learning: new）：[[idee]]、[[table]]、[[stable]]、[[parfait]]、[[maintenant]]、[[utile]]、[[bouger]]、[[un-peu]]、[[quoi]]、[[pratique]]、[[ne-plus]]、[[papier]]、[[moi-meme]]、[[fonctionnel]]、[[quelque-chose]]、[[main]]、[[mensuel]]、[[chez]]、[[le-mien]]、[[venir]]、[[enlever]]、[[diriger]]、[[embouteillage]]、[[guerre]]、[[peindre]]、[[avis]]、[[marteau]]、[[louer]]、[[debut]]、[[quantite]]、[[million]]、[[lave-vaisselle]]、[[donner]]、[[partout]]、[[en-train]]、[[voyager]]、[[dictionnaire]]、[[moutarde]]、[[lait]]、[[lumiere]]、[[toujours]]、[[bibliotheque]]、[[eux]]、[[essayer]]、[[drole]]、[[epouser]]、[[economiser]]
- **去重补充 5 处**（词条已存在，**未另建页**，均已在各页「来源」记复收录 + `updated` 2026-10-05）：`roi`（第 232 行）· `expliquer`（第 235 行）· `pris`→**[[prendre]]**（第 219 行，过去分词，lemma 归并）· `du bleu`→**[[bleu]]**（第 211 行，部分冠词）· `per erreur`→**[[erreur]]**（第 218 行，判定为 `par erreur` 拼写变体）
- **lemma 归并 4 处**（各页「收录说明」记录 inbox 原形）：`bouge`→[[bouger]]、`viens`→[[venir]]、`donne`→[[donner]]、`essaie`→[[essayer]]
- **判定待用户确认 4 处**：`plus partique`→**pratique**（拼写变体 partique→pratique）、`per erreur`→**par erreur**、`en papier` 按**名词 papier** 建页、`au lait` 按**名词 lait** 建页（后两处 lemma 取词典 headword）
- **格式自检（脚本，47/47 通过）**：frontmatter 13 项字段齐全（含 lang / gender / plural / word / ipa / pos / root）、每页**恰一个** `??` 分隔符、闪卡小节均带 `#flashcards/fr`、正文无游离 `?`；**47 页「来源」节的行号逐一核对与 inbox 实际行号一致**（子代理自行 `grep` 核实——brief 所给行号有 +2 位移，实际块为 199–250）
- **悬空链接核对**：本批**新增 0 条悬空 wikilink**（现存 4 条 comte / grand / lentille / vivre 均为法语侧旧有待建信号）
- **已勘正的 brief 偏差（矛盾显式记录，未静默覆盖）**：①**guerre**——brief 断言「英 war 与法 guerre 同源但**非**英借自法」；etymonline 明记英语 **war ← 古北法语（诺曼）werre ← 法兰克 \*werra**，即**确经诺曼法语中介**——页内两说并列、以权威源表述为准；②**stable**——brief「法语 stable 兼名词『马厩』」经 Larousse / Wiktionnaire / TLFi 核实**不成立**（马厩 = écurie / étable），系 **faux ami**；③**parfait**——brief「英经古法 perfet 借入」未获支持，etymonline 只记 **parfit**（ME c.1300）；④**fonctionnel**——brief「英 functional 系 19c 仿法语」**方向相反**：英 functional 首证 **1630s** 早于法 fonctionnel（ca 1830），实为**平行构词**；⑤**économiser**——brief「19c 英→法回流」**未见来源支持且年代不符**：英 economize 1640s 早于法 économiser 1718，且法语「省钱」义（1747）反早于英语（1790），判为**两语平行自造**（共出希腊 oikonomia）
- **首证并存诸说（不折中）**：marteau 三说（xiie s. / v.1140 / 1380）、moutarde（ca 1223 vs 12c.）、lave-vaisselle（1925 vs 1969）、louer 赞美义（fin 11e s. vs Xe s.）、toujours（ca 1170 vs 1er tiers xiie）、essayer（1080 vs xie s.）、lumière（约 1100 vs xie s.）、mensuel（1794/1795）；**词源诸说**：chez（casa 方位格 vs 复合 etymon）、drole（Verdam 中古荷兰 drol vs Sainéan \*tragulare）、moutarde（mustum+-ard vs mustum ardens）、table（拉丁 tabula 词源「不明」）
- **本批最值得记的一条**：**英法「同形异义」标本——法 main（手）vs 英 main（主要的）**；对照 **stable（法「稳定的」/ 英「稳定的」同义，但法**无**「马厩」义——马厩 écurie/étable）**——两条 faux ami 已入 [[french-vocabulary]] 命名消歧登记表
- **更新**：[[french-vocabulary]]（词条 181→**228**、待处理 +第十四批、词条列表 +47、**待回补 +第十四批（≈150 项）**、**命名消歧登记表 +2 行**（8 词英语侧 `-en` 预置 ＋ 4 处 lemma 判定待确认））；`index.md`（Stats：fr 181→**228**、总 310→**357**、wiki 页面 5→**6**；Backlog +法语第十四批待回补；**Answers 区 +[[article-partitif]]**）；log 本条

## [2026-10-05] query | 归档 raw/ 部分冠词文档 → wiki/answers/article-partitif.md

- **来源**：`raw/部分冠词（article partitif）.md`（用户新放入 raw/ 的整理稿，git untracked）——按 CLAUDE.md §3.2 第 5 条「好答案回填」归档为 `wiki/answers/article-partitif.md`（依 `templates/answer.md`）；`raw/` 原文**未改动**
- **内容**：部分冠词（article partitif）四形式 du / de la / de l' / des ＋ 与不定冠词之别（`c'est de l'art` vs `c'est un art`）；**依据节挂本库实例**：`du bleu`（→ [[bleu]]）· `au lait`（→ [[lait]]，并辨 **au = à + le 缩合 ≠ 部分冠词**）· `en papier`（→ [[papier]]，介词 en 表材料）；**后续节**留三个衍生问题（部分冠词 vs 缩合冠词 au/du/des 的分工、否定句部分冠词变 de、beaucoup de/peu de 后一律 de）
- **交叉**：本页与第十四批法语词条 [[bleu]]（页内 `du bleu` 复收录已链 [[article-partitif]]）、[[papier]]、[[lait]] 互链；补记 **待验证**：部分冠词的权威语法表述（CNRTL / Grevisse / Académie）本次未逐条对照
- **更新**：`index.md`（Answers 区 +1、Stats wiki 页面 5→6）；log 本条

## [2026-10-07] ingest | vocab: 英语第七批 5 词 → 5 新页（sand down / grenade / piecemeal / debris / sting）

- **动了哪些文件**：新建 `wiki/vocab/en/sand-down.md`、`grenade.md`、`piecemeal.md`、`debris.md`、`sting.md`（共 5 页）；更新 [[english-vocabulary]]（进度 129→**134**：待处理、词条清单、第七批待回补、词族候选 +3）；更新 `index.md`（Stats en 129→134、Backlog + 英语第七批待回补）；本 log 条
- **去重判定**：inbox 第 138–142 行为新增（`git diff` 核对）；**无重复页**——本批 5 词全库无近形冲突
- **关键决定 / 发现**：
  - **出处**：5 行**均未标注**——推断续前批语境（「From the book *Mattering*」等），**待用户确认**
  - **`sand down`**：etymonline **无短语条目**，故其自身首见年代**标待验证、未臆造**；拆查组件 sand（v.「砂磨」**1858**）+ down（late OE *ofdune* ← *of dune*「离开山丘」）；与既有 [[dry-up]] 的 up（「穷尽」）成小品词对照（up 向尽 / down 到底）
  - **`grenade`**：与 [[pomme]]（本批法语新页）**双向互见**——同一法语 *grenade*「石榴」在英语**先以「石榴」义入英（1520s）、再以「手榴弹」义入英（1590s）**，而水果义由 **pomegranate**（c.1300）承担 → **英语把「多籽果」一分为二**；根为拉丁 granum（PIE \*gre-no-）
  - **`piecemeal`**：**-meal 不是现代后缀**，而是古英语 *mæl*「时刻」的与格复数 **mælum**「at a time」——今只活在 piecemeal / inchmeal 等化石词里；**已勘正**「1590s/1580s/1560s」诸年代属 **inchmeal / piece 其他义、非本词**
  - **`debris`**：英 ← 法 *débris*（**1708**）；**briser 终极来源照录 etymonline 的推测措辞**（"possibly of Gaulish origin" / 另路 "probably of Celtic origin"）——**不当事实**
  - **`sting`**：**日耳曼本族词**（OE stingan ← PGmc \*stingan），PIE 终极来源 etymonline 明记 "**Perhaps**"（照录）
- **方法变更（重要）**：**etymonline.com 直连 WebFetch 本会话起返回 403**（转引代理亦被安全验证拦截）——本批英语词源**一律经 WebSearch 检索结果所引条目文本**取得，状态属「**转引**」而非「原条直核」；**cnrtl.fr 直连仍全 sinkhole**。已登记入两 hub 的「数据通道变更」条与 `index.md` Backlog
- **核实方式**：1 个子代理并行核实 5 词（etymonline 经检索转引；OED / M-W 对照部分未做，登待回补）
- **待办**：约 25 项待回补（无条目者 1 · 词源明记未知/推测者 2 · 细节待核 3 · 已勘正 brief 1）——见 [[english-vocabulary]] 第七批待回补条与各页「来源」节

## [2026-10-07] ingest | vocab-fr: 法语第十五批 37 条 → 35 新页 + 2 处去重

- **动了哪些文件**：新建 `wiki/vocab/fr/` 下 **35 页**——`decorer`、`cle-usb`、`reunion`、`installer`、`professionnel`、`original`、`acheter`、`outil`、`compter`、`machine-a-cafe`、`quelqu-un-d-autre`、`dossier`、`jusque`、`courrier`、`pomme`、`proprietaire`、`tout-le-temps`、`raquette`、`piscine`、`midi`、`detester`、`plat`、`epice`、`chance`、`rentrer`、`on`、`vendre`、`dentifrice`、`salle-de-bain`、`oiseau`、`peur`、`savon`、`monde`、`d-habitude`、`enorme`；更新 [[french-vocabulary]]（进度 228→**263**：待处理、词条清单、第十五批待回补、**命名消歧登记表 +1 行（11 词）**、词族候选 +7、数据通道变更、待建信号）；更新 `index.md`（Stats fr 228→263、Backlog + 法语第十五批待回补）；本 log 条
- **去重判定**：inbox 第 251–287 行为新增（`git diff` 核对）——**去重 2 处**：`au lait`（第 251 行，系第十四批第 242 行重出）→ 已在 [[lait]]；`quand`（重出）→ 页已存在，均未另建；**归并 4 处**（`decore`→décorer、`comptent`→compter、`deteste`→détester、`vend`→vendre）；**拼写变体 2 处**（`dentifirice`→dentifrice、`enorme`→énorme）
- **关键决定 / 发现**：
  - **lemma 判定待用户确认 1 处**：`decore`——去重音后与 *décoré* / *décore* **三形同形**，判为归并到 **décorer**（本批 inbox 混有屈折形，如 `comptent`），已在该页「收录说明」记录
  - **方向反转 / 仿借发现（重要）**：**[[professionnel]] 很可能是仿英语 professional 而造**（CNRTL 明言）——英 professional（形 mid-18c / 名 1848）**早于**法 1842 / 1872，属**法 ← 英**，已与「英语回借簇」候选合观
  - **两语独立直取拉丁**：[[dentifrice]]——英 *dentifrice* **early 15c 早于**法 1495，etymonline 明言英词系直取拉丁 *dentifricium* → 与法语**共祖而非借入**（**英法方向非法 → 英**）
  - **faux ami 集群（11 词同形 / 近形）**：已登命名消歧登记表；其中 **`on` 最须留意**——法语 **on 是代词**、英语 **on 是介词**，**同形异类异源**
  - **「同义异源」vs「同源异途」两层并列**：[[peur]] ↔ 英 fear（**祖源不同**：拉丁 *pavor* vs 日耳曼 \*feraz）｜ [[savon]] ↔ 英 soap（**同一 PGmc \*saipôn、路径不同**——既非借入亦非严格共祖）｜ [[oiseau]] ↔ 英 aviary（共祖，英直取拉丁）｜ [[salle-de-bain]] 的 *salle* ↔ 英 salon / saloon（同出日耳曼 \*salaz，非直接借 *salle*）——四型已登候选综合页
  - **勘正 2 处**：**[[dossier]] 文件义「1580」经复核无据 → 改 1586**（TLFi，Arch. mun. de Bayonne）；**[[proprietaire]] 的 1676 Ménage 记载系词形讨论、非首证** → 按 TLFi 记 13 世纪
  - **不采信 1 处**：**[[plat]] 的「法兰克语 \*flat」说**——fr.wiktionary 未列该形，本页不写入、标待验证
  - **助动词两制标本**：[[rentrer]]（不及物 **être** / 带宾语 **avoir**）
- **方法变更（重要）**：**cnrtl.fr 直连仍全 sinkhole**（多试只回「Portail lexical」外壳）**＋ etymonline 直连 403**——法语首证一律经「**fr.wiktionary 直取（引 TLFi / DMF / Académie 9e）+ WebSearch 让 TLFi 条文浮现**」取得，英语侧经检索转引 etymonline，**状态属「转引」**；清账通道排序已更新入 [[french-vocabulary]] 开放问题
- **核实方式**：2 个子代理并行核实 35 词（French 侧 fr.wiktionary 直取 + 检索转引；英语侧 WebSearch 转引 etymonline）
- **待办**：约 65 项待回补（首证并存两/三说 14 处 · 词源诸说并列 5 处 · 首证未获 5 处 · 已勘正 2 处 · 方向反转 2 处 · 未逐条核年若干）——见 [[french-vocabulary]] 第十五批待回补条与各页「来源」节
- **命名占位提醒**：本批法语页占了全库 basename `on`、`chance`、`dossier`、`installer`、`original`、`plat`、`monde`、`midi`、`piscine`、`dentifrice`、`reunion`——**英语侧日后收同形词须记 `-en` 后缀**（已登命名消歧表）

## [2026-10-11] ingest | vocab-fr: 法语第十六批 23 条 → 22 新页 + 1 处重出去重

- **动了哪些文件**：新建 `wiki/vocab/fr/` 下 **22 页**——`campagne`、`riviere`、`endroit`、`nord`、`ferme`、`vache`、`cochon`、`moustache`、`enfance`、`reve`、`frais`、`sale`、`propre`、`calme`、`emouvant`、`vieux`、`habiter`、`rappeler`、`utiliser`、`pres-de`、`la-bas`、`cela-fait-longtemps`；更新 [[trouver]]（第 304 行 `trouve` 重出补记，`updated` 2026-10-01→2026-10-11）；更新 [[french-vocabulary]]（进度 263→**285**：待处理、词条清单、第十六批待回补、**命名消歧登记表 +2 行（8 词 + 5 词）**、词族候选 +6、待建信号）；更新 `index.md`（Stats fr 263→**285** → 总 **419**、Backlog + 法语第十六批待回补）；本 log 条
- **去重判定**：inbox 第 288–310 行为新增（`git diff` 核对）——**重出去重 1 处**：`trouve`（第 304 行，系 2026-10-01 首发重出）→ 已在 [[trouver]]，**未另建页**、仅在页内「收录说明」补记；**lemma 归并 4 处**（`habite`→habiter、`rappelle`→rappeler、`utilisions`→utiliser、`vieille`→vieux）；**拼写勘正 4 处**（`emouvant`→émouvant、`reve`→rêve、`pres de`→près de、`la bas`→là-bas）
- **关键决定 / 发现**：
  - **同形多词判定 2 处**：`ferme` 判为 **n.f.「农场」**（同形 adj.「坚定的」/ v. fermer 收同页「同形异义」小节——**是否分页待用户确认**）；`vache` 兼收 adj. 俗义（*La vache !* / *vachement*）
  - **方向反转 / 反向借入（重要）**：**[[nord]] 系法语借自古英语 norþ**（经诺曼底，1140 首见）——英 **north 为本族词**，两词**同源、方向相反**（本库罕见标本）；**[[utiliser]] → 英语 utilize（1794，反向借入 fr → en）**
  - **同源双胞 / 同根借出**：**[[rappeler]] ↔ 英语 repeal**（同出古法 *rapeler*——一个成「废止」、一个留「叫回 / 提醒」）；**[[vache]] → 英语 vaccine**（同出拉丁 *vacca*）；**[[ferme]] → 英语 farm（c.1300 ← 古法 ferme「租金」）**；**[[riviere]] → 英语 river（early 13c ← 盎格鲁-法语 rivere）**；**[[campagne]] → 英语 campaign（1640s）**；**[[propre]] → 英语 proper / property / propriety**；**[[moustache]] → 英语 mustache（1580s）**
  - **同义异源对照**：[[vache]] ↔ 英 cow、[[cochon]] ↔ 英 pig / swine（英家畜词皆日耳曼——**唯 pork ← porc 借入**，诺曼分层标本）；[[sale]] ↔ 英 dirty、[[propre]] ↔ 英 clean——**两两同义异源**；[[enfance]] ↔ 英 child / childhood
  - **faux ami**：[[propre]]（干净 / 自己的）≠ 英 proper（**无「干净」义**，英语 = clean）；[[riviere]] 的 **rivière vs fleuve** 之分英语 river 一概不分（**高优先待收 fleuve**）
  - **本库罕见「多说标本」**：**[[reve]] 的 resver 源头 ≥6 说**（\*esver / \*reexvadare / \*reexvagus / \*reex-vare / revehi / \*refragu，另 rabies 已否）——**全部并列、绝不择一**
  - **已勘正 2 处**：**fermer 为 1er groupe（非 3e）**——原设误、已改；**[[propre]]「1080」诸源查无 → 已剔除**（保留 1090 vs xiie 两说）
  - **跨页互链**：[[habiter]] ↔ 英语页 [[habitat]]（**英 habitat 1762 系拉丁直造、早于法 1818**）
- **方法变更（沿用第十五批）**：**cnrtl.fr 直连仍全 sinkhole ＋ etymonline 直连 403**（fr.wiktionary 亦**部分被网络策略拦截**）——法语首证经「**WebSearch 转引 TLFi / DMF / Académie 9e / Robert 史典 / FEW / Usito**」、英语经**检索转引 etymonline**，**状态属「转引」而非「原条直核」**
- **核实方式**：**4 个子代理并行**核实 22 词（A 乡村名词 5 · B 生物名词 5 · C 形容词 6 · D 动词 + 短语 6）
- **待办**：约 45 项待回补（首证两 / 三说 8 处 · 词源诸说 6 处 · 首证未获 / 无条目 5 处 · 已勘正 2 处 · 方向反转 / 同源 5 处）——见 [[french-vocabulary]] 第十六批待回补条与各页「来源」节
- **命名占位提醒**：本批法语页占了全库 basename `sale`、`moustache`、`calme`、`propre`、`frais`、`nord`、`ferme`、`endroit`——**英语侧日后收同形 / 近形词须记 `-en` 后缀**（**`sale` 尤须注意：法语 sale 是「脏的」、英语 sale 是「售卖」，同形异类异源**；已登命名消歧表）
