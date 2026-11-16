# Datadog 遥测

## 这个来源里有什么

Datadog 存的是运行时记录，生产环境实际发生了什么，而不是计划或讨论过什么。

- **指标。** 团队埋的计数器、仪表值（gauge）、直方图。一个指标的*存在*本身就是证据。有人认为这个数值得盯。
- **监控器与告警。** 团队认定值得把人叫醒的条件。一个在 `rate_limit_hit > 10/min` 上触发的监控器，是团队担心那个阈值的直接证据。
- **仪表盘。** 精选视图。图表告诉你团队认为一个子系统里什么重要。
- **APM 链路与片段。** 请求级运行时数据。适合「这里为什么慢」「这里为什么超时」这类问题。
- **日志。** 大量事件记录。常包含促成防御性代码的错误状况。
- **事故。** 正式事故记录，带时间线和关联复盘报告。
- **笔记本。** 探索性调查。常包含假设与分析。

Datadog 回答「这段代码写成前后，生产环境的现实是什么」，这常常解释代码的形状。

## 怎么搜

用 Datadog MCP。先撒大网，再收窄。

1. **确认归属服务。**

   ```
   search_datadog_services (filter by name or team)
   search_datadog_service_dependencies (see upstream/downstream)
   ```

2. **先看仪表盘和监控器。它们告诉你团队在意什么。**

   ```
   search_datadog_dashboards (query: feature name, service name, symbol)
   search_datadog_monitors   (same queries)
   ```

   仪表盘或监控器覆盖到目标时，记下它的查询和盯的阈值。「为什么钳在 N」的答案常常就是那个阈值。

3. **目标周边的指标。**

   ```
   search_datadog_metrics (by name pattern, e.g., the feature or symbol)
   get_datadog_metric_context (metadata: description, units, tags)
   get_datadog_metric (timeseries; "was there a spike around the PR date?")
   ```

   把指标走势与目标的新增/修改日期对上，是有力的佐证。「`payment_timeout` 指标在 2023-11-03 飙升，重试逻辑于 2023-11-06 合并。」

4. **日志。收窄，别倾倒。**

   ```
   search_datadog_logs (raw log patterns near the target, set use_log_patterns=true)
   analyze_datadog_logs (SQL-style aggregations, only when you need counts)
   ```

   用符号、错误字符串或功能名搜。**强烈建议限定时间范围**（如改动前后各 30 天）。日志量巨大。不限范围的搜索浪费时间，还可能超时。

5. **APM 片段与链路。**

   ```
   aggregate_spans    (stats: "how often does this endpoint fail?")
   search_datadog_spans (inspect individual spans)
   get_datadog_trace  (a specific trace ID)
   ```

   适合超时、重试、慢路径和跨服务行为。

6. **事故。**

   ```
   search_datadog_incidents (by title, team, date range)
   get_datadog_incident     (full detail for a specific incident)
   ```

   目标看着像防御性代码时，搜它加入前后那段时间的事故。时间线里有 "added defensive check for X" 的事故，接近直接证据。

## 这里的好证据长什么样

- 一个监控器，其查询和阈值正好对上代码强制的约束（代码钳到 100，监控器在请求超过 100/min 时告警）
- 目标作者建的仪表盘，小组件对应代码所测量或防范的东西
- 一个指标在代码合并前夕出现生产尖峰，之后数值平稳
- 一条引用目标代码、相同符号或相同错误字符串的事故记录
- 日志显示防御性代码能防住的特定错误模式，时间戳落在改动前的窗口里

## 常见坑点

- **相关不是因果。** PR 前飙升、PR 后平稳只是提示，不是定论。同一窗口可能落了别的改动。查相邻的 PR。
- **对找到的图表过拟合。** Datadog 可视化是人*做*出来的，反映那个人的取景。一张叫 "retry success rate" 的图，证明团队在意重试成功率，不证明某行代码因它而存在。
- **消失的遥测。** 指标会被改名、删除，或保留期很短。找不到相关窗口的数据，那是缺口，不是空结果。
- **规模带来的噪声。** 拿常见字符串搜日志会返回几千条命中。狠一点，按服务、标签、时间收窄。用 `analyze_datadog_logs` 聚合，别倒原始日志。
- **埋了点 != 因它而起。** 指标存在只说明有人在意到要测量它，不说明代码*因为*它而加。与提交/PR 日期交叉核对。

## 返回什么

每条相关条目，附上：
- 类型（仪表盘 / 监控器 / 指标 / 日志模式 / 链路 / 事故 / 笔记本）
- 标题或名称
- 链接或标识（仪表盘 ID、监控器 ID、指标名、事故 ID）
- 所有者/作者与创建/修改日期
- 与问题相关的具体条件、查询或引文（尽量逐字）
- 相关性：它对目标代码提示了什么，关联有多强