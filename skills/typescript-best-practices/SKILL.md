---
name: typescript-best-practices
description: "读或改 TypeScript 代码时应用."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# TypeScript 最佳实践

先应用 **type-system-discipline** 原则技能。

| 规则 | 要点 |
|------|---------|
| 可辨识联合 | 用 `kind` 字面量判别字段给变体建模，让不可能的状态无法表示。不用可选字段大杂烩。 |
| 品牌类型 | 用 `& { readonly __brand: "X" }` 给原语打品牌标记，让它们混不了。在边界校验一次。 |
| 构造式建模 | 把形状搭成非法值构造不出来。非空用 `[T, ...T[]]`，偶数长度用 `[T, T][]`，区间用 `start` 加 `duration`。不是运行时守卫，也不是对精化类型的空想。 |
| 最简全类型 | 只要对它的每个操作都是全的（total，对任何输入都有定义），就保持 `T[]`。只有松类型逼出 `!`、强转或「绝不该发生」的 throw 时，才收紧成 `NonEmpty<T>`。 |
| `unknown` 优先于 `any` | 外部数据是 `unknown`。 |
| schema 先于守卫 | 手写逐属性的类型守卫之前，先用仓库的运行时 schema（模式）库，从 schema 推断类型，比如 `z.infer`。 |
| 不用 `as` 强转 | 每个 `as` 都是一次等着发生的运行时崩溃。校验之后才强转。 |
| 收窄层级 | 判别字段 switch > `in` 运算符 > `typeof`/`instanceof` > 自定义类型守卫 > `as`。 |
| 类型守卫 | 必须验证论断。说谎的守卫比 `as` 更糟，因为 bug 躲在一个声称安全的名字后面。命名用 `isX` 或 `hasX`。 |
| 穷尽性 | 在 default 分支里内联 `const _exhaustive: never = x;`，新增变体时编译器就会报错。 |
| `satisfies` 优先于 `as` | 校验值，又不放宽字面量类型。 |
| 边界校验 | 数据跨进来的地方就解析，解析成具名的领域类型。`Record<string, unknown>`（不管怎么拼）止步于那次解析。内部信任类型。见 **boundary-discipline** 原则技能。 |
| schema 派生类型 | 声明新接口之前，先用 `Pick`/`Omit`/`Parameters`/`ReturnType`/`Awaited`/`typeof`。 |
| 对象参数 | 传对象，不传位置参数，参数顺序自带说明。热路径上跳过（逐帧渲染、词法分析器、解析器）。 |
| 真实测试 | 能真跑的别用 mock（模拟）。优先用框架真实的测试原语，带泄漏/可释放检查，界面在跑起来的构建里验证。只 mock 本地跑不了的。 |
| 结构化遥测 | 优先用结构化日志诊断，上下文够从一个 id 查起。发布的代码里不写 `console.log`。 |

示例见 `references/patterns.md`。