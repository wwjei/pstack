---
name: principle-type-system-discipline
description: "设计类型或审查函数签名时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# 类型系统纪律

让编译器拒绝不可能的状态、不匹配的原语和未处理的变体。优先把错误和特殊情况定义到不存在，而不是到处加处理器。TypeScript 语法见 `typescript-best-practices` 技能。

- **给语义原语打品牌标记。** `UserId` 和 `OrderId` 底下都是字符串，但不该互换。给每个自己的类型，比如 TypeScript 的品牌化交叉类型，或别的语言里的 newtype（新类型）。创建时校验一次，下游信任类型。两个参数共享同一个原语类型却含义不同，就打品牌标记。
- **外部数据在解析前没有类型。** RPC 载荷、JSON、IPC 消息、CLI 参数、配置文件、环境变量、数据库行。每个边界放一个解析函数，把无结构输入变成带类型的模型。**boundary-discipline** 原则技能说了校验放哪。
- **从权威 schema 派生类型。** protocol buffer、OpenAPI 规格、GraphQL schema、数据库迁移、设计令牌文件或其他文件拥有某个形状时，从它派生，别手写一套平行类型。见 **encode-lessons-in-structure** 原则技能。