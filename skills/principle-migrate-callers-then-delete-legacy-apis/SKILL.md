---
name: principle-migrate-callers-then-delete-legacy-apis
description: "替换内部 API 时应用，先迁移再删除."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 先迁调用方，再删旧 API

- 更新测试去断言新契约，删掉只保护重构前实现细节的测试

**何时适用。**
- 没有外部用户依赖向后兼容
- 项目能承受协调一致的破坏性变更
- 新 API 是简化或重构计划的一部分