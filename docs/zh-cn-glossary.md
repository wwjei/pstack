# 中文术语表

本分支（`zh-cn`）把 pstack 的技能树译成中文。术语按下表统一，全文一致，不随语境换词。

## 翻译范围

`skills/` 下的全部 markdown 都译成中文，包括每个技能的 `SKILL.md`、`poteto-mode` 的 17 本操作手册，以及各技能挂的 `references/`。共 90 个文件。

`docs/guide/` 的 10 篇教程暂留英文原文，作为下一批。`README.md` 保留英文正文，只在开头加一节中文分支说明。

第一批已完成的 9 个文件：

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

其余 81 个文件为第二批，清单见本目录下的技能树。

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
| gate / staging / canary | 关卡 / 预发 / 灰度 | |

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
