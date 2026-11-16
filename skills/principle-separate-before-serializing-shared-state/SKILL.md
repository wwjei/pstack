---
name: principle-separate-before-serializing-shared-state
description: "并发执行体可能写同一状态时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 共享状态先分离，再谈串行化

并发执行体可能共享可变状态时，先问它们是否需要同一个可变对象。不需要就消除共享。共享是真的时，用结构强制串行化，锁文件、顺序阶段、独占所有权。指示和约定不是并发控制。

**为什么。** 对共享状态的并发写造成竞态条件，时隐时现，难复现，调试代价高。

**模式。**
1. **识别共享可变状态**（两边都读写的文件、两边都推的分支、两边既定义又消费的 API）。
2. **默认消除共享的写目标。** 问，这些执行体需要一个权威对象，还是在发布各自独立的事实？给每个执行体自己拥有的文件、键、分支或状态目录，只在读取/汇报边界合并。两个子代理各写自己的 `lastX` 字段进同一个 `state.json`，仍然是共享改动。`indexer-state.json` 加 `metrics-state.json` 就不是。
3. **只有当一个共享写目标是真实的不变量时，才用结构串行化访问**（锁文件、顺序阶段、单写者执行体，或原子比较并交换）。把「我们需要一把锁」当作要检查的设计坏味道，不是默认答案。