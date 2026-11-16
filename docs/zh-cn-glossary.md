# 中文术语表

本分支（`zh-cn`）把 pstack 的技能树译成中文。术语按下表统一，全文一致，不随语境换词。

## 翻译范围

`skills/` 下的全部 markdown 都译成中文，包括每个技能的 `SKILL.md`、`poteto-mode` 的 17 本操作手册，以及各技能挂的 `references/`。共 90 个文件。

`docs/guide/` 的 10 篇教程暂留英文原文，作为下一批。`README.md` 保留英文正文，只在开头加一节中文分支说明。

全部 90 个文件已译完。第一批 9 个核心技能：

| 文件 | 说明 |
|------|------|
| `skills/poteto-mode/SKILL.md` | 编排器，整套系统的入口 |
| `skills/poteto-mode/playbooks/authoring-a-skill.md` | 编写技能的操作手册 |
| `skills/how/SKILL.md` | 代码探索与讲解 |
| `skills/architect/SKILL.md` | 写代码前先设计 |
| `skills/interrogate/SKILL.md` | 对抗式审查 |
| `skills/swarm/SKILL.md` | 并行分发与汇总 |
| `skills/unslop/SKILL.md` | 去掉文字里的 AI 味 |
| `skills/deslop/SKILL.md` | 去掉代码里的 AI 味 |
| `skills/setup-pstack/SKILL.md` | 配置委派模型与推理预算 |

其余 81 个文件为第二批，已完成：24 个 `principle-*` 原则技能、16 本操作手册，以及 `why`、`reflect`、验证系列、`teach`、`technical-writing`、`typescript-best-practices` 等技能与它们的 `references/`。

## 保持原样，不翻译

这些是机器要读的标识符，改了会失效：

- frontmatter 的键名，以及 `name`、`version`、`author`、`license`、`platforms`、`tags`、`related_skills` 的值。只有 `description` 译成中文。
- 技能名与调用写法：`pstack:how`、`skill_view(name='pstack:architect')`。
- Hermes 工具名：`terminal`、`read_file`、`search_files`、`write_file`、`patch`、`delegate_task`、`clarify`。
- 文件名、路径、命令、代码块内容、git 术语原文（`merge base` 等首次出现时给中文对照）。
- 字面标记：`PASS / ISSUES / BLOCKED`、`ponytail:`、`inherit-parent`、配置键名。
- 一级标题保留技能标识名（如 `# How`），因为用户按这个名字调用。

## 术语对照

| 英文 | 中文 | 说明 |
|------|------|------|
| skill | 技能 | 指 SKILL.md 这一份可加载的规程 |
| orchestrator | 编排器 | `poteto-mode` 的角色 |
| playbook | 操作手册 | 一步步的执行流程，不译「剧本」 |
| routing | 路由 | 按任务类型选手册 |
| subagent / worker | 子代理 | 两者同指 `delegate_task` 派出的执行体，统一用「子代理」 |
| delegate | 委派 | 动词 |
| brief | 任务说明 | 交给子代理的那段交代 |
| brief（给用户的汇报） | 简报 | `recall`、`session-pickup` 等产出的那份汇报，跟交给子代理的 brief 不是一回事 |
| capsule / state capsule | 状态胶囊 | 最多 5 条的现状速览 |
| fan out | 并行分发 | 一次调用派出多个子代理 |
| slice | 切片 | 一份互不重叠的工作范围 |
| scope | 范围 | |
| intent | 意图 | 这次改动要达成什么 |
| done predicate | 完成判定条件 | 可判真假的收工标准 |
| adversary | 对抗审查者 | `interrogate` 里负责挑错的角色 |
| finding | 发现项 | 一条被运行证明的问题 |
| repro | 复现 | 复现脚本、复现步骤 |
| sketch | 草图 | 类型与签名的伪代码稿 |
| seam | 接缝 | 模块之间的边界处 |
| public surface | 公开接口 | |
| interface depth | 接口深度 | 调用方要用对，得先知道多少 |
| ladder | 阶梯 | 「要不要写这段代码」的逐级判断 |
| Laziness Protocol | 懒惰协议 | 保留「懒惰」的字面意思，指主动少写 |
| principle | 原则 | |
| non-negotiables | 硬性规则 | 触发即执行，无例外 |
| guardrails | 约束 | |
| autonomy | 自主权 | |
| irreversible | 不可逆 | |
| trust boundary | 信任边界 | |
| idempotent | 幂等 | |
| root cause | 根因 | |
| context window | 上下文窗口 | |
| reasoning budget | 推理预算 | |
| slop | AI 味 | 文字与代码通用；代码场景写「代码 AI 味」 |
| em dash | 破折号 | 指 U+2014 那个长横，本项目禁用 |
| partition / race / mix | 分区 / 竞速 / 混合 | `swarm` 的三种形态 |
| gap | 缺口 | 没拿到结果的切片 |
| gotchas | 坑点 | |
| working tree | 工作树 | |
| merge base | 合并基 | |

## 文风约定

沿用 pstack 自己的 `pstack:unslop` 规则，中文同样适用：

- 短句，陈述句。不写导语、不写功能巡礼。
- 不用破折号（U+2014），改用句号或逗号。CI 会检查。
- 不用冒号做句中连接词。
- 不用「不仅……而且……」「赋能」「打造」「助力」这类空话。
- 同一个概念只用一个词，不为求变化换同义词。
- 保留原文的判断和口气，包括「拒绝」「删掉」「先别写」这类直接命令。
- `description` 必须 ≤60 字符且以半角句点 `.` 结尾，这是 `scripts/check_refs.py` 的硬性检查。

## 术语对照（第二批补充）

| 英文 | 中文 | 说明 |
|------|------|------|
| evidence | 证据 | |
| proof | 证明 | |
| claim | 论断 | |
| verdict | 结论 | |
| verification | 验证 | |
| formal verification | 形式化验证 | |
| verifier | 验证器 | |
| harness | 验证工装 | 能驱动真实应用的现成装置 |
| feature map | 功能地图 | |
| transcript | 会话记录 | |
| session | 会话 | |
| decision log | 决策日志 | |
| drift | 漂移 | |
| ticket | 工单 | |
| owner | 所有者 | |
| investigator | 调查者 | `why` 里的角色 |
| reviewer | 审查者 | `reflect` 里的角色 |
| critic | 批判者 | `how` 批判模式里的角色 |
| synthesizer | 综合者 | 把多份结果合成一份的角色 |
| panel | 评审组 | |
| hillclimb | 爬坡 | 一轮轮小幅改进 |
| autopilot | 自动驾驶 | |
| babysit | 盯跑 | 守着长跑任务 |
| eval | 评测 | |
| benchmark | 基准 | |
| blast radius | 影响半径 | |
| probe | 探针 | 一次性的探路脚本 |
| exit predicate | 退出判定条件 | |
| gate | 关卡 | 必须停下来等人的点 |
| checkpoint | 检查点 | |
| queue | 队列 | |
| drain | 收齐 | 等所有子代理返回 |
| spec | 规格 | |
| pseudocode | 伪代码 | |
| signature | 签名 | |
| scaffold | 脚手架 | |
| caller / callee | 调用方 / 被调用方 | |
| entry point | 入口 | |
| edge case | 边界情况 | |
| regression | 回归 | |
| noise floor | 噪声底线 | |
| limiter | 限制因素 | |
| sample / spread | 样本 / 离散程度 | |
| invariant | 不变量 | |
| contract | 契约 | |
| state machine / registry | 状态机 / 注册表 | |
| early return | 提前返回 | |
| pass-through / shallow module | 透传 / 浅模块 | |
| sunk cost | 沉没成本 | |
| red flag | 红旗 | |
| ship | 发布 | |
| thread | 线索 | 一条工作线索 |
| trail / audit trail | 审计轨迹 | `show-me-your-work` 的决策留痕 |
| run | 运行 | 一次运行指一个代理对话，含其后续轮次 |
| epistemics | 证据分级 | `why` 的证据强弱框架 |
| citation | 引用出处 | |
| hypothesis | 假设 | |
| confidence | 置信度 | |
| source | 来源 | |
| `[Direct]` / `[Supported]` / `[Inferred]` / `[Speculative]` / `[Unknown]` | 直接证据 / 多源支撑 / 推断 / 猜测 / 未知 | 方括号标签本身保留英文，中文只用于正文解释 |
| status tag | 状态标签 | `[merged #N]`、`[open PR #N]` 等保留英文原文 |

## 来源类别对照

`why` 及其 `references/sources/` 全线统一用这套中文名，不许出现变体。

| 英文 | 中文 |
|------|------|
| source control history | 版本控制历史 |
| issue / ticket tracker | 工单系统 |
| long-form documents | 长文档 |
| real-time team chat | 团队实时聊天 |
| infrastructure observability | 基础设施可观测性 |
| error / exception tracking | 错误与异常追踪 |
| product analytics warehouse | 产品分析数仓 |
| code comments | 代码注释 |

## 报告小节名对照

报告小节名一律译成纯中文，不写「中文（English）」双语形式。只有方括号分级标签和状态标签保留英文。

综合者输出（定义在 `skills/why/references/synthesizer-prompt.md`）：

| 英文 | 中文 |
|------|------|
| The Question | 问题 |
| The Code in Question | 涉及的代码 |
| What We Found | 我们找到了什么 |
| What We Can Reasonably Infer | 我们能合理推断什么 |
| Competing Hypotheses | 相互竞争的假设 |
| What We Don't Know | 我们不知道什么 |
| Sources Consulted | 查过的来源 |
| Confidence Summary | 置信度小结 |

调查者输出（定义在 `skills/why/references/investigator-prompt.md`）：

| 英文 | 中文 |
|------|------|
| Source | 来源 |
| What I Searched | 我查了什么 |
| Direct Evidence Found | 找到的直接证据 |
| Indirect / Circumstantial Evidence | 间接证据与旁证 |
| Contradictions | 矛盾 |
| Gaps | 缺口 |
| Additional Leads | 额外线索 |

## 其他固定译法

| 英文 | 中文 |
|------|------|
| code anchor | 代码锚点 |
| coverage map | 覆盖地图 |
| seed context | 种子上下文 |
| confidence tiers | 置信度分级 |
| null result | 空结果 |
| recency bias | 近因偏差 |
| rationalization | 事后合理化 |
| sycophancy | 迎合 |
| counterfactual | 反事实 |
| circumstantial evidence | 旁证 |
| lead | 线索 |
| lineage | 沿革 |
| feature flag | 功能开关 |
| rate limiting | 限流 |
| circuit breaker | 熔断器 |
| hotfix / hot path | 热修复 / 热路径 |
| hedge | 留余地 |
| forcing function | 驱动因素 |
| paper trail | 书面记录 |
| egress guard | 出口守卫 |
| calibration | 校准 |
| roster | 名册 |
| Operating Posture | 工作姿态 |
| Preserve / Change / Avoid / Risk | 保留 / 改动 / 避免 / 风险 |
| gate / staging / canary | 关卡 / 预发 / 灰度 |
| chat thread | 聊天讨论串 | 跟工作线索（thread）和主线程（main thread）区分开 |
| Sub-features | 子功能 |
| How to get to it (user POV) | 用户怎么走到这里 |
| Driving it with control-notes | 用 control-notes 驱动 |
| Preconditions | 前置条件 |

## 输出字段与结论 token

`reflect` 的审查者与综合者共用一套字段名，全部译成中文：

| 英文 | 中文 |
|------|------|
| Principle | 原则 |
| Evidence | 证据 |
| Routing | 路由 |
| Reason | 理由 |
| Problem / Proposal | 问题 / 提案 |
| Accepted / Rejected / Backlog | 采纳 / 拒绝 / 待办 |
| Durability / Specificity / Convergence | 长效性 / 具体性 / 趋同性 |
| Existing-skill-first / Decision-changing | 现有技能优先 / 改变决策 |
| Structural-mechanism check / Skill-was-used / Already-covered | 结构机制检查 / 技能用过才算 / 已覆盖 |

反引号里的路由与拒绝 token 保留英文：`tune description:`、`new skill:`、`new skill via create-skill:`、`skill-not-used`、`already-covered`，以及理由枚举值 `durability | specificity | ...`。

验证技能的结论 token 保留英文，在定义清单处给一次中文对照，别处只用英文 token：

| token | 中文对照 | 出处 |
|-------|---------|------|
| `clean` / `changed` / `blocked` | 干净 / 有改动 / 受阻 | `maintain-verification-skill` |
| `in sync` / `updated` / `bug` / `blocked` | 一致 / 已更新 / 缺陷 / 受阻 | `maintain-formal-verification-skill` |

## 跨文件统一裁定

同一个英文词在不同批次被译成了两种说法，一律以下表为准。

| 英文 | 唯一译法 | 被弃用的变体 |
|------|---------|-------------|
| lane | 泳道 | 通道 |
| rubric | 评分标准 | 评判准则 |
| program | 项目（常设项目） | 计划、常设计划 |
| coordinator | 协调器 | 协调者 |
| workaround | 变通方案 | 绕路代码 |
| option value | 选择权 | 选择余地 |
| teardown | 拆除 | 拆解 |
| audit tick | 审计巡检 | 审计轮次 |
| chat thread | 聊天讨论串 | 聊天线程、消息串 |
| source control | 版本控制 | 源码控制 |
| issue tracker | 工单系统 | 问题跟踪器、工单跟踪器 |

近义词按来源词区分，不混用：

| 英文 | 中文 |
|------|------|
| plan | 计划 |
| workflow | 工作流 |
| workstream | 工作线 |
| decompose | 拆分 |
| race（`swarm` 形态） | 竞速 |
| race condition（并发缺陷） | 竞态条件 |
| trail / audit trail | 审计轨迹 |
| decision trail | 决策轨迹 |
| postmortem | 复盘报告 |
| reflect（技能） | 复盘 |
| issue（Linear） | 工单 |
| issue（Sentry） | 问题 |
| issue（GitHub） | 议题 |
| runbook | 运行手册 |
| playbook | 操作手册 |
| verify / verification | 验证 |
| validate / validation | 校验 |

操作手册标题译成中文，文件名保持英文：

| 文件 | 标题 |
|------|------|
| `investigation.md` | 调查 |
| `bug-fix.md` | 修 bug |
| `feature.md` | 新功能 |
| `perf-issue.md` | 性能问题 |
| `refactoring.md` | 重构 |
| `autonomous-run.md` | 自主长跑 |
| `figure-it-out.md` | 自己想办法 |
| `authoring-a-skill.md` | 编写或修改一个技能 |
| `opening-a-pr.md` | 开 PR |
| `shipping.md` | 发布 |
| `orchestrate.md` | 编排 |
| `babysit.md` | 盯跑 |
| `autopilot-full.md` | 全自动驾驶 |
| `autopilot-stack.md` | 堆叠自动驾驶 |
| `hillclimb.md` | 爬坡 |
| `eval.md` | 评测 |
| `session-pickup.md` | 会话接续 |

## 其他补充术语

| 英文 | 中文 |
|------|------|
| forge | 托管平台 |
| trunk | 主干 |
| stack / restack / upstack | 堆叠 / 重新堆叠 / 堆叠上层 |
| land | 落地 |
| countersign | 副签 |
| receipt | 凭据 |
| fix-forward | 向前修复 |
| flake | 偶发失败 |
| stale base | 基线过期 |
| load-bearing behavior | 承重行为 |
| triage | 分诊 |
| oracle | 判定基准 |
| one-way door | 单向门 |
| falsifiable predicate | 可证伪的判定条件 |
| stop predicate | 停止判定条件 |
| baseline / plateau | 基线 / 平台期 |
| judge / candidate / blinding | 裁判 / 候选者 / 盲测 |
| sanitized | 脱敏 |
| codemod | 代码批改器 |
| sweep | 批量修改 |
| fixture / stub / mock | 夹具 / 空桩 / 模拟 |
| seed data / disposable | 种子数据 / 一次性 |
| doctor | 健康检查 |
| recipe | 配方 |
| source wave / live pass | 源码波次 / 实跑轮 |
| project-local | 项目本地 |
| surface | 交互面 |
| property / bound / counterexample | 性质 / 界限 / 反例 |
| model checker / prover / proof assistant | 模型检查器 / 证明器 / 证明助手 |
| upkeep loop | 保养循环 |
| learning / durable learning | 教训 / 长效教训 |
| lens | 视角 |
| parent agent | 父代理 |
| missed-trigger | 漏触发 |
| real home | 真正归宿 |
| incident / monitor / dashboard | 事故 / 监控器 / 仪表盘 |
| trace / span | 链路 / 片段 |
| instrumentation | 埋点 |
| gauge | 仪表值 |
| discriminated union / discriminant | 可辨识联合 / 判别字段 |
| branded type / narrowing / type guard | 品牌类型 / 收窄 / 类型守卫 |
| exhaustiveness / total type | 穷尽性 / 全类型 |
| wire format / wire types | 传输格式 / 报文类型 |
| primitives | 原语 |
| accidental complexity | 偶然复杂度 |
| design smell | 设计坏味道 |
| temporal decomposition | 按时间分解 |
| load generator / release build | 负载生成器 / 发布构建 |
| hotspot / ballpark | 热点 / 粗估 |
| tutorial / how-to / reference / explanation | 教程 / 操作指南 / 参考 / 解释 |

## 原则名对照

`principle-*` 技能的目录名保留英文，正文里的原则名一律用下表的中文，跟 `poteto-mode` 的原则小节完全一致。

| 技能目录 | 中文原则名 |
|----------|-----------|
| `principle-laziness-protocol` | 懒惰协议 |
| `principle-foundational-thinking` | 地基式思考 |
| `principle-attack-the-premise` | 攻击前提 |
| `principle-subtract-before-you-add` | 先减再加 |
| `principle-minimize-reader-load` | 降低读者负担 |
| `principle-outcome-oriented-execution` | 面向结果执行 |
| `principle-experience-first` | 体验优先 |
| `principle-build-the-lever` | 造杠杆 |
| `principle-model-the-domain` | 给领域建模 |
| `principle-boundary-discipline` | 边界纪律 |
| `principle-type-system-discipline` | 类型系统纪律 |
| `principle-make-operations-idempotent` | 让操作幂等 |
| `principle-separate-before-serializing-shared-state` | 共享状态先分离，再谈串行化 |
| `principle-prove-it-works` | 证明它能用 |
| `principle-fix-root-causes` | 修根因 |
| `principle-sequence-verifiable-units` | 按可验证单元排序推进 |
| `principle-test-behavior-not-implementation` | 测行为，不测实现 |
| `principle-explain-the-number` | 解释那个数字 |
| `principle-guard-the-context-window` | 守住上下文窗口 |
| `principle-never-block-on-the-human` | 绝不卡在等人上 |
| `principle-exhaust-the-design-space` | 穷尽设计空间 |
| `principle-migrate-callers-then-delete-legacy-apis` | 先迁调用方，再删旧 API |
| `principle-redesign-from-first-principles` | 从第一性原理重新设计 |
| `principle-encode-lessons-in-structure` | 把教训编码进结构 |

## 技能名对照

技能名本身不译，下表只用于正文里需要解释它是什么的场合。

| 技能 | 中文说法 |
|------|---------|
| `poteto-mode` | 编排器 |
| `how` | 代码讲解 |
| `architect` | 设计 |
| `interrogate` | 对抗式审查 |
| `swarm` | 并行分发 |
| `unslop` | 去文字 AI 味 |
| `deslop` | 去代码 AI 味 |
| `setup-pstack` | 配置 pstack |
| `automate-me` | 把我自动化 |
| `recall` | 回溯 |
| `reflect` | 复盘 |
| `sharpen` | 磨利 |
| `teach` | 教学 |
| `why` | 追意图 |
| `show-me-your-work` | 摊开过程 |
| `in-your-own-words` | 用你自己的话 |
| `figure-it-out` | 自己想办法 |
| `bro` | 说人话 |
| `blast-radius` | 影响半径 |
| `benchmark-checklist` | 基准清单 |
| `no-comments` | 不写注释 |
| `tdd` | 测试驱动开发 |
| `technical-writing` | 技术写作 |
| `typescript-best-practices` | TypeScript 最佳实践 |
| `create-verification-skill` | 生成验证技能 |
| `maintain-verification-skill` | 维护验证技能 |
| `create-formal-verification-skill` | 生成形式化验证技能 |
| `maintain-formal-verification-skill` | 维护形式化验证技能 |
