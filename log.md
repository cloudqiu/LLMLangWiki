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
