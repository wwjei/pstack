---
name: benchmark-checklist
description: 汇报结果前先核验基准证据.
version: 1.0.0
author: poteto (original Cursor plugin)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [performance, measurement]
    related_skills: [principle-explain-the-number]
---

# Benchmark checklist

汇报一个基准之前，核验限制因素、调优、极限、错误、可重复性、相关性，以及那个工作真的发生了。把上游 pstack 文档里的详细清单当作操作指引。每个数字都记下运行次数、离散程度、限制因素和证据。