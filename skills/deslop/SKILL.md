---
name: deslop
description: 提交前去掉 diff 里的代码 AI 味.
version: 1.0.0
author: Cursor team kit (Cursor), ported for Hermes
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [cleanup, code quality]
    related_skills: [pstack:unslop]
---

# 去掉代码里的 AI 味

对着 main 检查 diff，清掉这个分支引入的代码 AI 味。用 `terminal` 拿 diff（`git diff main...HEAD`），用 `patch` 做修改。

## 重点看这几处

- 多余的注释，或者跟本地风格不一致的注释
- 在可信代码路径上反常的防御性检查或 try/catch
- 只为绕过类型问题而转成 `any` 的强制转换
- 该用提前返回来简化的深层嵌套
- 其他跟本文件及周边代码库不一致的写法

## 约束

- 除非在修一个明确的 bug，否则保持行为不变。
- 优先做最小、聚焦的修改，不做大范围重写。
- 最终总结保持简短（1 到 3 句）。

`pstack:unslop` 对文字做同样的事。这个技能管代码。
