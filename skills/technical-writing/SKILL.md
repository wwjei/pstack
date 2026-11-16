---
name: technical-writing
description: "写或改技术文档、README 时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 技术写作

目标是让疲惫的工程师第一遍就读懂。四层做到这一点，每层回答一个问题。这是什么类型的文档，句子如何称呼读者，每句话承载多少，有没有句子能读出两种意思。四层全部用上。

三条规则在各层之上：

- **删掉每个不干活的词。** 去掉一个词句子还成立，这个词就得走。"In order to" 就是 "to"。"It is important to note that" 什么都不是。
- **用短的日常词。** 用 "Use"，不用 "utilize"。用 "Help"，不用 "facilitate"。用 "Do"，不用 "perform"。长词得用精确性挣回它的长度。
- **规则让句子变差时，换个方式修，或者别动它。** 规则为读者服务。一条规则都守了、读起来却像机器写的句子，就是失败的。

代码库就是词表。写真实的符号、文件、标志或命令名，不写同义词，也不写对它的描述。

别发明行话。用开发者会说出口的词："move"、"delete"、"a budget that only decreases"（只会减少的预算），不用 "evacuate"、"ratchet"、"endgame"。有名字的模式可以用，前提是文档第一次出现时就说清它指什么。发现新的违规词，就在回复里连同替换词一起提议补进 `unslop` 的抽象隐喻规则，附上 diff。别改那个技能。

## 变换节奏

各层决定文档说什么、每句话承载多少。一份文档可以全守了，读起来仍像机器写的。每句都剪得短，处处没观点，没有任何具体的东西。

- 有意混用句子长短。短句把一个点砸实。慢慢来的长句承载一个事实，连同它的条件或后果。
- 一句一个想法，不等于一句一个长度。承载两个想法的句子拆开。只承载一个想法的长句留着。
- 模式允许的地方要有观点。解释要权衡取舍，所以说出你的判断，别罗列优缺点。参考保持干巴巴。
- 要具体，不要无菌。不写 "schema changes can cause issues"，写 "a column rename fails the build"（改列名会让构建失败）。

## 先选模式（Diátaxis）

一份文档，一种模式。两个问题选定它。内容是指导行动（做）还是帮助理解（想），它服务于学习还是工作。

- 行动 + 学习：**教程**。
- 行动 + 工作：**操作指南**。
- 理解 + 工作：**参考**。
- 理解 + 学习：**解释**。

这个罗盘可以用在整份文档上，也可以用在单个句子上。

**教程：边做边学。** 你是老师。学习者能不能学会是你的责任，不是他们的。开头就说学习者会做出什么，不说他们会「学到」什么。每一步都产出可见的结果，要早、要多。告诉他们应该看到什么：预期输出、提示符变化、日志行。解释压到一个从句加一个链接。教学停顿会打断课程。保持具体。用「我们」来写，用命令句："First, do x. Now, do y."（先做 x，现在做 y。）

**操作指南：达成目标的步骤。** 解决人遇到的问题，不是机器能执行的操作。假定读者有能力。跳过教学。只写行动。不跑题，不铺背景，不为完整而完整。那些放链接。允许分叉和判断："If you want x, do y."（想要 x，就做 y。）按任务给指南命名："How to calibrate the radar array"，不是 "Radar array calibration"。

**参考：供查阅的事实。** 描述。只描述。不指导，不说服，不发表意见。干巴巴、完整、确定。陈述事实、选项、限制和错误，不带含糊限定。结构镜像被描述的东西，让代码和文档能对照着浏览。材料放在读者预期的位置。能从代码生成就从代码生成，让它保持真实。

**解释：理解与缘由。** 一个有边界的话题，脱离产品也能读。每个标题前面都应容得下一个隐含的 "About..."（关于……）。锚定一个真实的「为什么」问题。给出上下文。设计决策、历史、约束、备选方案。观点只允许出现在这里，别处不行。

别混模式。教程里不放参考表格，参考里不做教程式手把手，操作指南里不论证。拆开，用链接连起来。

## 句子写给读者（Google 开发者风格）

- 对读者称 "you"，用现在时。"Will" 只用于真正之后才发生的事。
- 说清谁做什么："the compiler checks"，不是 "is checked"。只有施动者未知或无关紧要时，被动才可以。
- 指令写成命令："Click Submit."。事实平铺直叙。绝不写 "should be done"。
- 条件放在指令前："To delete the document, click Delete."。读者会跳过不适用的部分。
- 常见情况放前面。例外放后面。
- 听起来像个懂行的朋友。不用热词，不用比喻，指令里不写 "please"，步骤里绝不写 "simply"、"easy"、"quickly"。真那么简单，读者就不会在这里了。
- 不预告（"we will soon support..."），相邻句子不用同一个短语开头。
- 链接文字要说清链接去哪：页面标题或一句短描述。绝不写 "click here"。宁可在页内给一句上下文，也别放一个跳出去的链接。
- 标题承载要点，不只是话题（"Pick the mode first"，不是 "Modes"）。句首大写。任务标题用光杆动词短语（"Create an instance"）。概念标题用名词短语。每页一个 h1，不跳级。
- 顺序用编号列表，其余用圆点列表。列表前用一个完整句子引出。各项保持并列结构。
- 代码用代码字体。界面元素加粗。用序列逗号。去掉 "etc."，提前说明列表不完整。

## 每句话只装一件事（STE 规则）

- 一句一条指令。其他地方一句一个想法。
- 指令超过约 20 词就拆，其他句子超过约 25 词就拆。
- 警告或条件放在它守护的步骤之前："If hot oil touches your skin, injuries can occur."。
- 保留 "the" 和 "a"："Remove backup file" 有两种读法，"Remove the backup file" 只有一种。
- 每个词只给一个意思、一份工作，然后一直用它。"check" 若是检查的意思，就别再拿它表示抑制。
- 每个动作挑一个词，用到底："start"，别这里 "start" 那里 "initiate"。
- 步骤写成直接命令，不写成叙述，绝不用被动："Install the component"，不是 "the component must be installed"。
- 能避开 "-ing" 形式的词就避开。它们承担的语法角色太多，容易生出误读。

## 不给句子留两种读法（Global English）

- "only"、"not" 这类词紧贴它们修饰的词："only fails on growth" 和 "fails only on growth" 说的是两回事。
- 拆开长名词串："the proto import budget check script" 改成 "the script that checks the proto-import budget"。
- 每个 "it"、"they"、"this" 都指向一个明确的对象。拿不准就重复名词。绝不用 "this" 或 "which" 指代整个从句。
- 别省动词："Phase 1 moves the converters and Phase 2 the runtime" 让 Phase 2 没了动词。给它补上。
- 保留显示结构的小词。"Ensure that the switch is off" 留着 "that"，因为它让句子只有一种解析。绝不拿清晰度换字数。
- 并列时重复冠词能防误读就重复：两个东西就写 "the client and the host"，不写 "the client and host"。
- 句子有两种分组方式时，说清 "and" 或 "or" 连接的是哪几部分。"Both...and"、"either...or"、"if...then" 是免费的消歧手段。
- 用句号，不用分号。破折号（U+2014）换成一个新句子。
- 括号里的文字要么是完整语法单位，要么独立成句。绝不用 "(s)" 构成复数。
- 不用斜杠：写 "a, b, or both"，不写 "a/b" 或 "and/or"。
- 每个东西在任何地方都只叫一个名字。一份文档对同一个东西一会儿 "the gate"、一会儿 "the ratchet"、一会儿 "the budget check"，教会读者的是三个东西。两次编辑之间给没变的句子换措辞，代价一样。没变的东西别折腾。
- 跳过习语、口语、拉丁缩写和隐喻。非母语读者、译者和代理，都是平实结构解析得最好。

## 语气与仓库细节

- 本技能碰到的每份文档都过一遍 **unslop** 技能。AI 味套路清单归它管。AI 词汇、填充词、含糊限定、格式上的马脚。
- PR 描述和提交信息也是写作。除 Diátaxis 外的每一层都适用。PR 正文是一份简报，审查者一分钟内能读完。别贴 swarm 日志、SHA 清单或指标表格。放链接。
- 产品界面文案不是文档。那些按你产品的文案规范来。
- 代码片段用 tab 缩进。写真实路径、真实符号。每个计数或目录树论断，在落地它的那个提交上都必须为真，并附上重新生成它的命令。

## 改写示例

改写前：

> Configuration of the proto import ratchet budget script parameters is performed via budget.json. Note that it's important to remember that running with --write, which updates the committed budget to reflect the current count, should only be done when lowering it. If exceeded, CI fails.

改写后：

> `budget.mjs` reads the committed budget from `budget.json` and counts the files that import protos. If the count exceeds the budget, CI fails. Run `budget.mjs --write` only to lower the budget.