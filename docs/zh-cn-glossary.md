# 中文术语表

本分支（`zh-cn`）把 pstack 的核心技能译成中文。术语按下表统一，全文一致，不随语境换词。

## 翻译范围

已译为中文的文件：

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

其余技能、操作手册和 `docs/guide/` 仍是英文原文。

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
