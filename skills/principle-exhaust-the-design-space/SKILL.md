---
name: principle-exhaust-the-design-space
description: "新颖的 UI 或设计没有先例时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 穷尽设计空间

当一个新的交互或架构决策没有现成先例，实现之前先探索几个具体的备选。造错东西比探索三个方案更贵。

**规则。** 正确答案不明显时，造 2-3 个相互竞争的原型或草图。把它们并排比较。然后才定案。「设计两遍」就是这条规则的另一个名字。第一个形状的第二种口味不算数。

**什么时候适用：**
- 新颖的 UI 交互（代码库里没有先例）
- 有多条可行路线的架构选择
- 用户体验靠手感而不是逻辑的产品设计决策

**什么时候不适用：**
- 模式已确立的机械实现
- 目标状态清楚的 bug 修复或重构
- 约束决定了只有一条可行路线的改动