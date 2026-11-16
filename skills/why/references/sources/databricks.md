# Databricks 分析与系统表

## 这个来源里有什么

Databricks 是产品分析、数据管道和仓库遥测这一层。它与 Datadog 互补。Datadog 是*基础设施/运行时*视角，Databricks 是*产品/数据*视角（用户做了什么、跑过哪些实验、功能使用如何演变、某个阈值常量从哪来）。

- **产品分析事件。** `your_warehouse.events.analytics_track_event`（原始表）和 `<your_analytics_db>.<schema>.<table>` 里按事件分表、带类型、已去重的 dbt 模型。用户行为：功能调用、点击、接受/拒绝、提交、客户端上报的错误。
- **用量与计费事件。** `your_warehouse.events.usage_event` / `<your_analytics_db>.<schema>.stg_usage_events`、`your_warehouse.events.raw_model_event` / `<your_analytics_db>.<schema>.stg_raw_model_events`。用于成本或用量驱动的决策。
- **实验 / 功能开关数据。** 曝光表和结果表。**表结构因公司而异。** 先用 `SHOW TABLES` 探一探，别假设表名。
- **系统表。** `system.query.history`、`system.compute.warehouses`、`system.billing.*`、`system.access.audit`。回答「这条查询贵不贵」「有没有人经常跑它」「仓库负载什么时候飙升」。
- **dbt 血缘。** `<your_analytics_db>.<schema>` 里的模型能看出哪些管道依赖某张表/字段。上游变更经常促成消费方代码的改动。
- **Databricks 笔记本。** 工程师在改代码前写的探索性分析。**SQL MCP 查不到。** 若怀疑理由在某个笔记本里，把它报成缺口。

## 怎么搜

用 Databricks SQL MCP。主工具是 `execute_sql_read_only`。若返回 `statement_id`，用 `poll_sql_result` 轮询，不要重跑。

**先摸清再查。** 表结构因公司而异。先探明再信任表名：

```sql
SHOW TABLES IN <your_analytics_db>.<schema> LIKE '*<keyword>*';
DESCRIBE TABLE <your_analytics_db>.<schema>.stg_<event>;
```

**每条查询都限定时间。** 这些表极大，不限范围的扫描会超时。按 `_timestamp`（事件表）或 `start_time`（`system.query.history`）过滤，窗口罩住发布日期，通常前后各约 30 天，有充分理由才放宽。

**优先用带类型的 dbt 模型，别用原始表。** `<your_analytics_db>.<schema>.<table>` 已去重、带类型、做了 liquid 聚簇。`your_warehouse.events.analytics_track_event` 有重复，`properties_json` 没类型。模型命名模式是 `stg_<source>_<event_name_with_underscores>`，`<source>` 取 `app`、`backend`、`website` 或 `cli`。光靠模式定不下来时，用 `SHOW TABLES` 确认确切模型名。只有还没有 dbt 模型，或需要 dbt 刷新延迟之内的最新事件时，才降到原始表。

**带类型 dbt 模型的列约定**（记住这些能省一次 `DESCRIBE` 往返）：

- `_timestamp`、`_id`、`_auth_id`、`_request_id`、`event_name`。每个模型都有
- `properties_<name>`。带类型、下划线命名的各事件属性（`properties_entrypoint`、`properties_size_bytes` 等）
- `context_team_id`、`context_client_version`、`context_country`、`context_client_os`。预先抽取的客户端上下文

### 常有回报的调查套路

按目标选表加列的组合：

1. **事件使用轨迹。** 在 PR 合并前后 ±30 天窗口里，对相关 `stg_*` 模型按天计数。合并后一两天内从零跳到稳定量的阶跃，是 PR 上线了该功能的有力间接证据。衰减到零则提示弃用或删除。
2. **护栏 / 防御性检查的出处。** 看 PR *之前* 14 天里相关 `properties_<name>` 列的分布（中位数 / p99 / 最大值）。p99 恰好等于目标里的阈值常量，提示这个数是从数据里选的。
3. **实验 / 功能开关查找。** 用 `SHOW TABLES ... LIKE '*experiment*'` 找曝光表，再按变体拉出相关开关键在 PR 日期附近的曝光量。
4. **迁移、回填或性能重写的查询历史证据。** 在 `system.query.history` 上按 `statement_text ILIKE '%<table_or_symbol>%'` 过滤，配一个紧的 `start_time` 窗口，能浮出可能促成这次改动的高开销查询（按 `total_duration_ms` 排序，或聚合 `SUM(read_bytes)`、`COUNT(*)`）。
5. **dbt 血缘。** 若目标读写某个 `<your_analytics_db>.<schema>` 模型，该模型自己的 git 历史（在本仓库里）常常带着理由。把这条线索交回给 git 调查者，别自己追。

## 这里的好证据长什么样

除了上面那些套路形态：

- 某个错误分类事件的计数，在防御性代码 PR 之后几天内跌到接近零。提示该 PR 解决了那类错误
- 曝光表里有一行写着目标的功能开关键，在 PR 发布日期前后带着「shipped」/「concluded」的决定

## 常见坑点

- **埋了点 ≠ 因它而起。** 一个事件存在，只说明有人在意到要记录它，不说明目标代码*因为*它而存在。先配上 git 调查者给的 PR/提交引用出处，再下因果论断。
- **无声的埋点变更。** 事件量的阶跃可能只是新事件开始上报，不是用户行为变了。把爬升读成功能上线信号之前，先查同一窗口有没有埋点 PR。
- **表结构漂移。** 事件属性会演化。今天带类型 dbt 模型上的某列，目标代码写成时可能还不存在。旧数据可能只在原始 `properties_json` 里带这个属性。
- **dbt 刷新延迟。** `<your_analytics_db>.<schema>.*` 按计划重建（常是每小时/每天）。最近几小时的事件，退回 `your_warehouse.events.*` 并按 `_id` 去重。
- **公司专属的表。** 实验、功能开关、计费、用量表各家不同。从没确认过存在性的表里报结果，是经典失败模式。先用 `SHOW TABLES` / `DESCRIBE TABLE` 探。
- **保留期悬崖。** 若相关窗口早于表的保留期或 dbt 模型的创建日期，那是*缺口*，不是空结果。明确说出来，免得综合者把「没结果」读成「没活动」。
- **笔记本查不到。** SQL MCP 看不见 Databricks 笔记本。若怀疑理由在某个笔记本里，返回缺口。

## 返回什么

每条相关发现项，附上：
- 类型（产品事件 / 实验曝光 / 用量或计费事件 / 系统表行 / dbt 模型）
- 全限定表名和你跑的确切查询
- 查询的时间窗口
- 紧凑的数字摘要（计数、分位数、首见/末见时间戳）。**别倒原始行。**
- 与目标发布日期的时间相关性（如「首行 2024-08-15，PR #49074 于 2024-08-14 合并」）
- 相关性 + 强度：直接证据 / 间接证据 / 弱