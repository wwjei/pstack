---
name: principle-experience-first
description: "产品、体验或范围取舍出现时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 体验优先

实现的省事与用户的愉悦冲突时，选愉悦。

- 每个功能、控件、选项都必须有理由
- 少发布，发布得更好（三个功能打磨好的体验，胜过十个功能的粗糙版本）
- 定案前先做原型（设计决策在用完即弃的 HTML 里比在生产代码里便宜）
- 把细节做对（过渡、对齐、间距、反馈、错误状态）
- 收紧核心循环（每个功能要么服务于中心工作流，要么别挡路）

用户就是消费这份工作的人。对 UI 来说是最终用户。对库或内部 API 来说是导入它的同事。下一个维护这段代码的工程师也是用户。用同样的分量权衡他们的体验，并从他们的视角解释影响。

地基应当服务于体验。地基式思考管的是工作的*顺序*。这条原则管的是*目标*。