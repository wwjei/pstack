---
name: principle-subtract-before-you-add
description: "新增或重构时应用，先移除复杂度."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 先减再加

演进一个系统时，先移除复杂度，再构建。

**为什么。** 往复杂系统上加东西，复杂度会复利。先减留下更少的代码，露出本质结构，通常让下一步设计显而易见。默认做减法。

把简化当成持续投资。离开时让设计比你发现它时略简单、略能干，表面相同或更小。

**模式。**
- 移除排在构建之前
- 先砍再打磨（先到最小，再投资质量）
- 为观察到的用法设计，不为投机的边界情况设计
- 规格没要求的投机校验器、解析器、守卫，不写
- 简化提示词（删冗余指令、过度模板）
- 参考文件没有新内容时删掉，别留个空桩