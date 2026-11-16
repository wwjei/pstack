---
name: principle-never-block-on-the-human
description: "可逆工作想先请示时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 绝不卡在等人上

人是异步监督的。代理必须保持不被卡住。做合理的决定，推进，让人事后纠偏。

**为什么。** 每次停下来请示都让流水线停摆，让人成为瓶颈。代码改动可逆、可审，错误决定的代价通常低于卡住不动。

**模式。**
- **先做，再摆出来。** 把活干了，把结果摆出来。别问「要不要做 X？」做 X，解释为什么。
- **让系统自愈。** 发现问题就记下来，下一轮修掉。

**边界。**
- **不可逆动作**（强推、删生产数据、对外发消息）仍然要确认。
- **可逆动作**（写代码、改笔记、拆任务）应当直接推进，不卡住。
- **产品方向**由人定。*执行*不该卡住。