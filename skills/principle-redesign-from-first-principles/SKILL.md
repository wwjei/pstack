---
name: principle-redesign-from-first-principles
description: "给既有设计加需求时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 从第一性原理重新设计

整合一个改动时，别把它硬拧在既有设计上。重新设计，就当这个需求从一开始就在。

- 读完所有受影响的文件，理解当前设计
- 问，「如果带着这个新需求从零写，我们会造什么？」
- 把改动传播到每一处引用，类型、文档、示例、理由小节
- 想清楚整个重新设计，然后增量交付

这是在既有设计里整合改动时，保住选择权的方法。