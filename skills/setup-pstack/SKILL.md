---
name: setup-pstack
description: "选定 pstack 委派用的模型与推理预算."
version: 0.1.0
author: poteto (Hermes adaptation)
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [engineering, workflow]
    related_skills: []
---

# Setup pstack

配置每一次 pstack `delegate_task` 调用所用的模型和推理预算。pstack 自己没有配置文件。它写的是 Hermes 原生的 `delegation` 设置，由 `pstack:poteto-mode`、`pstack:swarm`、`pstack:interrogate`、`pstack:architect` 派出的每个子代理读取。

## 步骤

### 1. 读取当前状态

在 `terminal` 里跑：

```bash
hermes config get model
hermes config get delegation
```

记下父会话模型（`model.default`、`model.provider`），以及当前的 `delegation.model`、`delegation.provider`、`delegation.reasoning_effort`。值为空表示子代理继承父会话。

### 2. 探测可用模型

列出用户实际能路由到的模型。用 `hermes model` 的输出、provider 的 `/v1/models` 接口，或者配置里已有的模型。一个都确认不了时，请用户把他有的 slug 贴过来。绝不写入没有确认过的模型。`inherit-parent` 永远有效：它的意思是把 `delegation.model` 留空，让子代理跑在父会话模型上。

### 3. 问预算

用 `clarify` 给这四个选项。`delegation.reasoning_effort` 已有值时，把当前预算说出来。

- `unlimited (max)`
- `large (xhigh)`
- `medium (high)`
- `small (medium)`

括号里的值就是要写入的 `delegation.reasoning_effort`。

### 4. 问委派模型

用 `clarify`。先给 `inherit-parent`，再给探测到的模型。展示当前值。如果选中的模型需要跟父会话不同的 provider，把那个 provider 也问清楚（它必须已经在 Hermes 里配好）。

### 5. 写配置

```bash
hermes config set delegation.reasoning_effort <effort>
hermes config set delegation.model <slug>        # inherit-parent 时跳过
hermes config set delegation.provider <provider> # 仅当与父会话不同时
```

选 `inherit-parent` 时，清掉旧的固定值：`hermes config unset delegation.model` 和 `hermes config unset delegation.provider`。重复运行这个技能会覆盖同样的键，所以它是幂等的。

### 6. 核实

跑 `hermes config get delegation`，检查每个写入的键读回来都是所选的值。有值没生效，就报出来并停下。

### 7. 确认

告诉用户写了什么，以及它对新会话生效。评审组类技能（`pstack:interrogate`、`pstack:architect`）的多样性来自独立的子代理和不同的提示词。Hermes 会让一次调用里的所有子代理跑在同一个委派模型上，所以想要多模型评审组，就得在两次运行之间改 `delegation.model`。

### 8. 提供一条验证路径（可选）

看看项目有没有办法驱动真实应用来取证：一个 `verify-*` 技能，或者现成的验证工装（e2e 套件、冒烟脚本）。两样都没有时，说一次，并建议写一个，好让代理能像用户那样证明改动。不要推销。

### 9. 提供形式化验证技能（可选）

看看项目是否已经有形式化验证技能（一个跑模型检查器、证明器或其他验证器来对着代码检查的项目技能）。没有的话，提一次：「要不要一个形式化验证技能，让代理能证明你代码的某条关键性质在范围内的每一种情况下都成立，而不只是测试覆盖到的那些情况？我可以用 `pstack:create-formal-verification-skill` 生成一个。」答要，就加载那个技能。答不要，就往下走，不再推销。
