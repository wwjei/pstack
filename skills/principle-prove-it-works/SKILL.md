---
name: principle-prove-it-works
description: "任务做完后、宣布完成前应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 证明它能用

每个任务的产出都要直接查真实的东西来验证。不要从替代品、自我报告或「能编译」来推断。

**为什么。** 未验证的工作正确性未知。间接验证（文件修改时间、输出新鲜度、代理自我报告、缓存截图）感觉比直接观察便宜。基于错误推断去行动，代价远高于查源头。

查真实的东西，不查替代品。
- 直接查进程是否活着，不通过派生状态间接查
- 读实际的值，不读缓存或派生的表示
- 验证失败时，先怀疑观察方法，再怀疑系统

## 能把检查写成脚本就写

最有力的证明是一个确定性脚本，重跑同样的比对，而不是一次性的目测。写脚本，跑它，把输出留成产物，让审查者能重跑，而不是信你一句话。

把产物摆出来给人看。只有大型或复杂的工作才提交它，比如大移植或迁移，这类痕迹日后必须可审计（**show-me-your-work** 技能）。