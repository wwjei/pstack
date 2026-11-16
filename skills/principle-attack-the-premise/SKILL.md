---
name: principle-attack-the-premise
description: "共享同一前提的修复反复过不了关卡时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 攻击前提

当两个以上共享同一前提的修复都没过同一个关卡，怀疑前提，不是怀疑修复。

**为什么：** 每一次失败的修复都是反对它所假设的那个信念的证据。在同一前提上再做第三个修复，是把力气花在证据已经表明原因不在的地方。

**模式：**
- **把前提写下来。** 前提就是每个失败修复都假设的那一句话。
- **在下一次修复之前先检验前提。** 找一个观察，如果前提为假，它的结果会不同，然后去做这个观察。看失败是怎么分布的，不只看它们有多大。按[造杠杆](../principle-build-the-lever/SKILL.md)把检查做成可重跑的。
- **跟着证据走。** 如果证据与前提矛盾，原因在别处。去找失败的共同点里前提没预测到的东西。如果证据证实了前提，按[修根因](../principle-fix-root-causes/SKILL.md)问下一个「为什么」。之前的修复可能治的是后果，不是原因。
- **消除那个条件，而不是补偿它**，按[懒惰协议](../principle-laziness-protocol/SKILL.md)。如果证据显示某个部分总是落在输的一侧，改掉把它放在那里的东西。一个替它收拾残局的机制会让原因留在原地，还给每次运行加活。

**停：** 前提没写下来、没检验之前，不许开始下一次修复。

这条原则不同于[从第一性原理重新设计](../principle-redesign-from-first-principles/SKILL.md)，后者围绕一个新需求重建设计。它质疑的是当前设计假设的一个事实。