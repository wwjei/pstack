---
name: principle-sequence-verifiable-units
description: "多步骤工作适用，逐单元验证后再推进."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 按可验证单元排序推进

把工作排成一串小单元，每个单元结束在一个能检查的状态，当前单元不绿就不推进。

**为什么。** 在肇事单元当场抓到的破坏，定位便宜。攒成一批之后才发现的破坏被埋住了，而且你已经在坏地基上继续盖了。把同样这些单元排成审查者能重放的交付，就把「信我」变成「看着它先红后绿」。

**执行。** 批量修改、迁移或任何一串相似编辑中，每个改动先验证再开始下一个。每个单元是一组前后对照。已知良好的状态，一次改动，跑检查，然后继续。先变基（rebase）到干净的主干，让每次检查都对着真实基线测量。杠杆来做编辑时，逐单元检查几乎免费。照样要跑。

**交付。** 提交和 PR 按证明工作的顺序叠。标准形状是先放失败的测试，修复叠在上面。其他的故事顺序有，先减再重塑，先采基线再处理，先脚手架再功能。每个提交独立落地，整个序列读起来像一个论证。

它在排序上补齐两个原则技能。**prove-it-works** 让每次检查保持真实，**build-the-lever** 让逐单元检查便宜。