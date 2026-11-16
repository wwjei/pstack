---
name: show-me-your-work
description: "长跑任务需要留下决策轨迹时使用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# Show me your work

只留一份权威日志。

## 格式

一个 TSV 文件，一行一个决策。单元格保持单行。证据是指针，不是散文。

复制 `references/decision-log-template.tsv`（表头行）来起一份干净日志。列：

- **ts。** ISO8601 时间戳。
- **phase。** 阶段或工作流。
- **decision。** 选了什么或做了什么，一行。
- **why。** 用大白话写理由。如果是某条原则驱动的，直白说出来，不要写成一个术语标签。
- **evidence。** 能证明它的链接或路径：提交 SHA、PR 号、`file:line`，或者某个产物、trace、截图的路径。绝不是一段话。
- **result。** 结果或判定条件状态：`tests green`、`reverted`、`pixel-diff 0`、`INCONCLUSIVE`、`open`。

一个例子，写得平实，让审查者一眼能读。

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	counted the work first, about 100 components and roughly 75 hours	wanted to know the size before starting a long run	commit 3a9f1c2	found 5 things to sort out before starting
2026-05-24T09:40:00Z	harness	took screenshots of the old version before changing anything	so we can compare old against new and catch any visual change	scripts/snapshot.sh, baseline/	saved 120 reference screenshots
2026-05-24T11:15:00Z	widget	moved the widget styles over without changing how it looks	keep the change small and the result identical	commit 7c21e0a, pixel-diff 0	looks identical, tests pass
2026-05-24T12:30:00Z	widget	threw out a helper's work because its screenshots were blank	checked the real files instead of trusting its summary	worktree reset	reverted, tightened the instructions for next time
```

## 记一行

每一条都按你跟队友讲「我做了什么」的方式写。大白话，具体动作，不要 AI 腔和抽象术语（**unslop** 技能同样管日志文字）。

用辅助脚本 `scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`。它会盖 `ts`，首次使用时写表头，剥掉多余的制表符和换行，并给任何以 `=`、`+`、`-`、`@` 开头的单元格加一个单引号前缀。直接用 `printf` 追加一行也行，但单元格来自生成内容或用户输入时，要当心同样这几个字节。

记决策点和检查点，不是记每个动作：选了哪个分岔、一个单元完成及其验证结果、一次转向或回滚及其触发原因、冒出来的阻塞、修好的一个关卡。循环运行则一轮一行。琐碎和不言自明的跳过。

一次运行指一个代理对话，包含它后续的轮次和任何对它的总结。接续、换代理、开新对话，都算新的一次运行。当一次运行往已有行的日志里追加时，它的第一行 phase 为 `start`；在另一次运行的 `start` 行之后，它的第一行也是 `start`。所以一次在后续轮次回到日志的运行，要先读日志最后几行，看是否有别的运行写过。`start` 行要点名它前面那些不是本次运行写的行的 `ts` 区间，它的 evidence 要点名本次运行，比如它的代理 id。phase `start` 不作别的用途。

## 放在哪

默认日志是工作产物，不提交。放在工作目录的 `decisions.tsv`，几件事同时推进时用 `.audit/<task-slug>.tsv`，不要进 git。

只有当这份工作大到审查者需要轨迹才敢信结果时，才提交它。

## 规则

- 只追加。判断错了就新加一行取代它。绝不编辑或删除历史。
- 优先用已提交脚本产出的证据，而不是手工一次性凑的（**encode-lessons-in-structure** 原则技能）。

## 拿会话记录审计日志

运行结束、交回之前，检查日志说的是不是真话。用 `session_search` 工具读本次运行的会话记录，范围限定本次运行的会话。绝不用 glob 扫原始会话记录文件，也绝不伸手到别的 profile 的会话历史。把本次运行的行逐条对着实际发生的事走一遍。每一段行从本次运行的某个 `start` 行开始（如果日志是本次运行创建的，就从第一行开始），到别的运行的下一个 `start` 行结束：

- 检查每一行都对应一个真实的决策或动作。
- 检查每一行的 evidence 能解析，并且显示的内容跟该行声称的一致。
- 一个塑造了这份工作却没被记下来的分岔、转向或放弃的方案，就是缺口。补上。

修日志，不修故事。审计绝不编辑或删除任何一行，哪怕是编造出来的行。当某行既不是真实决策也不是真实动作，或者它的声称或证据是错的，就加一行取代它，写上实际发生了什么，以及一个能解析的指针。这次审计不检查本次运行各段之外的行。如果本次运行自己的工作表明其中某行是错的，就像对待任何错误判断那样取代它。

## 跨模型审查轨迹

交回之前，派一个子代理，跑在跟干活那个不同的模型家族上。自审不算替代。这个子代理读审计轨迹和本次运行的会话记录，然后标出用户该注意的地方。不是重做这份工作，是扫一遍哪里不够好、哪里有风险。

- 记下来但证据很弱或没有证据的决策。
- 被跳过、或者声称做了但会话记录里没有证明的验证步骤。
- 事后看着有风险的选择（做早了、范围膨胀、糊住症状）。
- 用户随手一翻会漏掉的缺口。

产生了轨迹的运行，每一条回复都以「注意」小节结尾。第一行单独写审查者的模型（`reviewed by <model>`），然后逐条列出标记，指向具体的行或时刻。「没有标记」是有效取值。模型名不是。

## 怎么看这份轨迹

从上往下读，跟着证据指针走，抽查。GitHub 会把提交了的 TSV 渲染成表格。终端里用 `column -s$'\t' -t decisions.tsv` 渲染。

## 别的技能怎么复用它

其他技能把它们的审计轨迹路由到这里，而不是自己发明一套。按名字引用它，格式归它管。不要复述那些列。
