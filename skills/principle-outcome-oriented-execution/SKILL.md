---
name: principle-outcome-oriented-execution
description: "有明确阶段关卡的计划内重写时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 面向结果执行

为意图中的、可验证的终态优化，而不是为保住平滑的中间状态。

**为什么。** 让每个中间步骤都完全稳定，往往造出临时兼容代码，变成长期债务。收敛到目标架构，在明确的验证边界上证明正确性。

**核心规则。**
- 终态完整性优先于过渡期稳定性
- 中间破坏只要是计划内、有范围、可逆的，就可以接受

**约束。**
- 用于有明确阶段边界的计划内重写和迁移
- 声明哪里可以接受临时破坏
- 迁移期间对正在动的区域保留高信号检查
- 计划完成时要求完整的静态和运行时验证