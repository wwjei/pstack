---
name: no-comments
description: "用 Comment Sicko 删掉多余的代码注释."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# No comments

派出 Comment Sicko。对接受的发现项动手。

听从 Comment Sicko 的新鲜视角。

## 范围

用调用方给的文件或 diff。否则用当前相对基分支的 diff，基分支默认 `main`，包含工作树。

## 步骤

1. 派一个 `delegate_task`，带上范围和 Comment Sicko 的审查提示词。把范围传过去。不要复述它的规则。
2. 检查它的报告和 diff。驳回应用代码编辑、范围逃逸、对受例外保护内容的删除、理由写错的 `MUST KILL`，以及把有意保留的代码当有罪的标记。针对自家代码意外处的重塑标记仍然要处理。不要恢复那些注释。保留只有在证明它涉及我们改不了的东西时才成立。审计范围内漏掉的 lint 和 TypeScript 抑制。涉及正确性或安全的抑制仍是要处理的 `MUST KILL`。只有给出确切例外和范围内证明，才恢复被删的注释。接受理由单薄的 `IMPORTANT` 或 `do not remove` 的杀或留之前，先对它的符号跑 `/how` 或 `/why`。杀有歧义，不恢复。留被驳倒或仍有歧义，删掉。驳回一份报告时，回滚并重跑一次，点名失败之处。第二次再驳回，就报告为未决，并让 `/no-comments` 失败。
3. 琐碎的已接受标记直接修，删死路径、去掉参数，或改用真实 API。任何修复需要定形状时，对已接受的那组和周边代码跑一次 `/architect`。停在草图。设计定形状。第 4 步实现。
4. 在范围内实现最小的根因修复。移除每一个点名的变通方案。根因在范围外时，落地范围内最小的修复，其余报告为未决。**principle-fix-root-causes** 和 **principle-redesign-from-first-principles** 技能只指引意图。两者都不授权扩围栏，也不授权修围栏外的实例。绝不外挂症状守卫。
5. 约束注释写着 `do not remove`、`do not change wording` 或 `talk to X before changing`。涉及我们改不了的东西的保留，留着。提出范围内最便宜的类型、运行时、测试或 CI lint 方案。等交互批准。无人值守和评测需要调用方预先批准。批准了就先编码进结构再删。否则删掉，把约束报告为未决，并给范围外的工作画草图。
6. 汇报删除数量、恢复的注释、重跑、设计草图、修复、编码提议、已编码项、未强制执行的约束，以及其他未决工作。