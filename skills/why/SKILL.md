---
name: why
description: "用于「X 为什么是这样」或「当初为什么选 Y」."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# Why

调查代码背后的动机与意图。

与 `how` 技能配套。`how` 回答代码做什么、怎么工作。`why` 回答是哪些力量让它长成这个形状。

下面每个调查者和综合者都跑在 Hermes 配置的 `delegation.model` 上（由 `pstack:setup-pstack` 设定）。`delegate_task` 没有按任务覆盖模型的选项，所以一轮之内做不到模型多样性。

## 工作姿态

以**仔细、谨慎、精确的调查者**的姿态工作。知道什么、推断什么，如实区分。读 `references/epistemics.md`，里面有完整的置信度框架和措辞指南。综合者必须照它执行。

## 第 1 步：理解目标与问题

解析用户在问什么。**目标**通常是一段代码、一个模式、一个功能，或一个点名的设计决策。**问题**通常是设计理由、一处取舍、一个促成改动的边界情况、一条外部约束、一段死代码，或一次大范围的历史梳理。

如果目标含糊（「我们为什么这么做？」却没有明确所指），就从会话上下文（打开的文件、最近的编辑、光标位置、刚讨论的内容）做出最佳猜测。简短说出你的理解，猜偏了用户好纠正，然后继续。

## 第 2 步：建立代码锚点

派出调查者之前，先把调查锚定在具体代码上。你需要：

- 相关文件路径和行范围
- 关键符号（函数名、类名、常量）
- 初始提交清单。最近几次触碰目标的提交。
- 合并提交里的 PR 号（标题行里的 `(#1234)` 模式）

就地构建。

```bash
# Blame target lines for last-touch commits
git blame -L <start>,<end> <file>

# Full file history, with patches, through renames
git log --follow -p -- <file>

# Last N commits touching the file, PR numbers visible
git log --oneline -20 -- <file>

# Extract PR numbers from a commit message
git log -1 --format=%B <commit>
```

对任何有实质内容的提交，用 `gh` 拉 PR 正文和讨论：

```bash
gh pr view <number> --json title,body,author,createdAt,mergedAt,labels,closingIssuesReferences,comments,reviews
```

把这些记成种子上下文（文件路径、符号、提交、PR 号、关联工单 ID）。传给调查者。

## 第 3 步：并行派出调查者（默认姿态）

**默认做完整的并行调查。**

### 发现

派出调查者之前，先列出 Hermes Agent 环境里可用的 MCP。有可用工具映射就用它。否则检查 Hermes Agent 暴露的 `mcps/` 目录，看启用了哪些 MCP 服务器。

把每个可用 MCP 映射到一个证据类别：

1. 版本控制历史
2. 工单系统
3. 长文档
4. 团队实时聊天
5. 基础设施可观测性
6. 错误与异常追踪
7. 产品分析数仓

版本控制永远可用，走 git 和 `gh`。其余六类，用 MCP 名称、服务器说明、工具名和资源描述来归类。一个 MCP 能归进多个类别时，选与它主要证据匹配的那个。模棱两可的情况记进覆盖地图。

目标是完整的**覆盖地图**，不是最小覆盖。空档也要记录，不要跳过搜索。

把所有匹配的调查者放在一条消息里发出，让它们并发跑。不要让一个代理覆盖多个 MCP。

子代理配置（每个都一样）：
- `toolsets`：`generalPurpose`
- 代理模式，不是只读。**不要用只读模式。**只读会剥掉 MCP 访问，让依赖 MCP 的调查者整个失效。调查者仍然不该写任何东西。

每个调查者拿到：
1. `references/investigator-prompt.md` 里的基础提示词
2. 所选 MCP 对应的类别操作手册 `references/sources/<source>.md`，参照 `references/source-playbook.md` 里的示例改写
3. 跨类别的 `references/sources/incident-postmortem.md`，**前提是目标代码看起来是防御性的**（空值检查、重试逻辑、超时处理、限流、功能开关、出口守卫、OOM 处理器）
4. 第 2 步的代码锚点（文件路径、符号、提交哈希、PR 号、工单 ID）
5. 用户的原始问题

### 调查者名册。每个可用证据类别一个

每个有匹配 MCP 的类别派一个调查者。每个只拥有一个工具或 MCP。

每条名目写明类别，以及它独有能挖出的那种「为什么」。用它来知道该期待什么返回、某个类别返回空时怎么命名缺口，以及（仅在极少数可证明无关的情况下）为跳过给出理由。

1. **版本控制调查者**。Git 历史、用 `gh` 查 PR、代码注释、测试。永远派出。唯一保证有的来源。最擅长挖出*评审时记录下来的实现期理由*。

2. **工单系统调查者**（如 Linear、Jira、GitHub Issues、Plane、Shortcut MCP）。最擅长挖出*产品或业务的驱动因素*。当「为什么」在工程之外时最强。

3. **长文档调查者**（如 Notion、Confluence、Google Docs、Coda MCP）。最擅长挖出*长篇设计理由*。「为什么」在变成代码之前被写下来的地方。

4. **团队实时聊天调查者**（如 Slack、Discord、Microsoft Teams、Mattermost MCP）。最擅长挖出*从未进文档的实时讨论*。当版本控制、工单和文档的书面记录很薄时尤其重要。

5. **基础设施可观测性调查者**（如 Datadog、New Relic、Honeycomb、Grafana、Splunk MCP）。基础设施 / 运行时视角。最擅长挖出*促使代码写成的基础设施与运行时现实*。当目标是对基础设施信号（超时、重试、限流、熔断器）的反应时最强。

6. **错误与异常追踪调查者**（如 Sentry、Rollbar、Bugsnag、Airbrake MCP）。最擅长挖出*促使防御性或纠正性代码写成的具体异常与错误轨迹*。对 catch 块、空值守卫、类型检查、重试和其他防御最强。

7. **产品分析数仓调查者**（如 Databricks、Snowflake、BigQuery、ClickHouse、dbt、Redshift MCP）。产品 / 数据视角。最擅长挖出*塑造代码的产品与数据现实*。对开关控制的代码、实验驱动的发布、数据迁移，以及「这个数字哪来的」这类问题最强。

### 什么时候跳过调查者

只有写出**明确的书面查过的来源」小节。两个有效理由：

- **该类别在这个环境里没有可用的 MCP**。把这标为缺口，不是选择。示例：「跳过团队实时聊天。没有匹配的 MCP，会话记录无法搜索。」
- **该来源可证明无关**，不是「可能无关」。门槛很高。示例：「跳过错误与异常追踪。目标是构建期脚本，没有运行时路径。」

如果范围评估表明这是单提交的琐碎目标，PR 描述里已有完整答案，你可以直接回答，**但必须先确认**七个可用类别的搜索全都是冗余的。明确说出来。这种情况应该很少。

## 第 4 步：综合

派一个综合者子代理：

- `toolsets`：`generalPurpose`
- 代理模式，不是只读。综合者的质量检查要抽查引用，可能需要 MCP 访问。只读模式会剥掉 MCP，让检查失效。

综合者拿到：
1. 调查者的发现项，包括空结果，以及带理由跳过的类别
2. 第 2 步的代码锚点（文件路径、符号、提交哈希、PR 号、工单 ID）
3. 用户的原始问题
4. `references/epistemics.md` 里的证据分级框架（标签 `[Direct]`、`[Supported]`、`[Inferred]`、`[Speculative]`、`[Unknown]`）
5. `references/synthesizer-prompt.md` 里的综合者提示词模板

## 第 5 步：呈现

拿综合者的输出呈现给用户。可以为清晰做轻微编辑，或补充会话里的上下文，但**不要改写置信度措辞**。

## 输出格式

输出结构就是 `references/synthesizer-prompt.md` 问题、涉及的代码、我们找到了什么、我们能合理推断什么、相互竞争的假设、我们不知道什么、查过的来源、置信度小结。按需调整，但保持置信度分层不变，查过的来源保持每个调查者一行，包括返回空的和被跳过的，附上原因。

查过的来源块之后，如果用户的 `why` 问题是要动手改这段代码的前奏，就把沿革发现转成一套「保留 / 改动 / 避免 / 风险」约束集，供规划这次改动使用。

## 要避免的常见失败模式

- **近因偏差**。假设最新的提交就是权威。当前形态往往是许多早期决策层层累积的结果。往回追。

## 参考文件

- `references/epistemics.md`。证据分级与措辞指南。综合者必须照它执行。
- `references/investigator-prompt.md`。调查者子代理的基础提示词模板。
- `references/source-playbook.md`。指向下面各类别操作手册的索引。
- `references/sources/*.md`。每个类别一份自包含的示例操作手册，外加跨类别的 `incident-postmortem.md`。给调查者与其类别匹配的那一份，并按可用 MCP 改写。
- `references/synthesizer-prompt.md`。综合者子代理的提示词模板，含输出格式。