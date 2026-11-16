---
name: reflect
description: "用户要求复盘时，把教训路由进技能."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# Reflect

从当前对话里挖出长效教训，再把它们路由成技能修改。

## 何时调用

用户说「reflect」或「/reflect」时调用。对话琐碎、跑题，或内容已被父代理正确遵循的现有技能覆盖时，跳过。一次性的事不算教训。

## 流程

### 1. 定位当前会话记录

父代理用 `session_search` 工具找自己的对话，限定在当前会话（`session_id` 取自系统提示词，用 `around_message_id` 读取，拉出完整范围）。绝不用通配符扫原始会话记录文件，也不去翻别的 profile 的会话历史。查询解析不出来时，写一份紧凑的会话摘要传过去代替。

### 2. 并行派出三个审查者

一次 `delegate_task` 调用带三个任务，用代理模式（不是只读模式：审查者需要 MCP 权限来查会话记录引用的上下文，比如工单、聊天讨论串或可观测性链路。只读模式会剥掉 MCP）。

| 视角 | 提示词模板 |
|---|---|
| 判断 | `references/judgment-reviewer.md` |
| 工具 | `references/tooling-reviewer.md` |
| 发散 | `references/divergent-reviewer.md` |

同一次 `delegate_task` 调用里的每个任务都跑在 Hermes 配置的 `delegation.model` 上（由 `pstack:setup-pstack` 设置）。没有按任务覆盖模型的机制，所以三个视角的多样性来自提示词，不是模型。要真正的模型多样性，就把三个视角分成三轮跑，每轮之间更换 `delegation.model`。

每份模板原样传递，只在标记处替换成会话记录内容或摘要。审查者把发现项放在任务结果里返回。

### 3. 综合

一次 `delegate_task` 调用，代理模式（不是只读模式：综合者的质量检查会抽查引用，可能需要 MCP 权限）。除非你在轮次之间刻意改过，它跑在与审查者相同的 `delegation.model` 上。原样使用 `references/synthesizer.md`，在标记处内联每个审查者的完整输出。综合者返回结构化的 采纳 / 拒绝 / 待办 清单。

### 4. 结构性强制检查

对综合者的 采纳 清单做合理性检查。任何一条由 lint 规则、脚本、元数据标记或运行时检查来强制会更可靠的，把它从 采纳 挪到 待办。参见**把教训编码进结构**原则技能。

### 5. 应用

应用任何 采纳 修改之前，先把综合者完整的 采纳/拒绝/待办 输出呈给用户，等明确批准。用户挑选要应用的子集，也可以改路由。技能改动会影响组织里之后的每一个代理。不要自动应用。

待办 项自动归档到团队用的 devex / 待办跟踪器。只有 采纳 清单需要等批准。

绝不改已安装插件目录里的技能（例如 `~/.hermes/plugins/<plugin>/skills/`）。下次插件更新会覆盖它。批准前，展示它的去处：当前 profile 的技能目录（`~/.hermes/skills/<skill>/`，命名 profile 下则是 `~/.hermes/profiles/<profile>/skills/<skill>/`）。批准后，把技能文件夹复制过去，改副本。绝不开 PR，也不推送。

对每条获批的 采纳 项，严格照 路由 字段执行：

- 现有技能的琐碎修改（一行要点、一句收紧的话、纠正一个过时事实）：父代理直接做。
- 现有技能的实质修改（新小节、新的模式表、超过约 10 行）：交给 Hermes Agent 内置的 `create-skill` 技能，跑它的起草 / 测试 / 迭代循环。
- `tune description: <skill path>`（技能存在，但该触发时没触发）：交给 `create-skill`，跑它的 description 优化循环。
- `new skill via create-skill: <kebab-name>`：创建交给 `create-skill`。不要临时自创形态。

环境自带 SKILL.md 校验器的话，宣布完成前对每个动过的技能跑一遍。没有就跳过这一步。

### 6. 向用户汇总

短清单，不写导语：

- 已应用的修改：`<skill path>`。改了什么，每条一行。
- 新建的技能：`<skill path>`。每个一行（少见）。
- 归档到 devex 跟踪器的 待办：`<issue title>`（`<tags>`）。每条一行。
- 丢弃：每条被拒发现项一行，附综合者给的原因。