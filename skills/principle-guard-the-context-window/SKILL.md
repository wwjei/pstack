---
name: principle-guard-the-context-window
description: "上下文被大输出或大文件填满时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 守住上下文窗口

上下文窗口有限，在一个会话内不可再生。每个 token 都应当值回它的成本。

**为什么：** 上下文溢出会降低推理质量，制造压缩失真，并让进度停摆。

**模式：**
- **隔离大负载。** 把冗长的输出、截图、大文档路由给子代理。主上下文拿摘要，不拿原始数据。
- **常用内容留在行内。** 每次调用都要用的模板和参考资料属于技能文件本身，不是单独的文件，那样每次都要花一次读取。
- **给阶段定尺寸，给范围封顶。** 限制每个阶段的文件数，设轮次预算，把机制本身的成本算进去。