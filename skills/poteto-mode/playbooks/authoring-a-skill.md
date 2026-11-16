### 编写或修改一个技能

**这个技能的语气由你负责。**

1. 用 **create-skill** 技能（Hermes Agent 内置的 SKILL.md 编写技能）。
2. 校验这个技能：frontmatter 有 `name` 和 `description`，被引用的文件都存在，跨技能链接都能解析。
3. 结构性的内容配测试用例。主观性的内容跳过。
4. 跑 **开 PR** 操作手册：`skill_view(name='pstack:poteto-mode', file_path='playbooks/opening-a-pr.md')`。

拿不准就删。只留能改变决策的文字。直接叫它去做那件事，理由省掉。只有当一条规则不给理由就令人困惑时，才解释。语气匹配范围。按 **encode-lessons-in-structure** 原则技能的做法，指向结构性的来源（类型、README、配置）。委派给别的技能时给路径。不要复述。一个你反复撞上、却还没被记下来的流程，就提议建一个新技能。

**回复：** 技能摘要、关键设计决策、校验说明。
