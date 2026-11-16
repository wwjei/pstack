# 来源操作手册

`why` 技能为每个可用的证据类别派一个调查者，各读下面一份来源专属的操作手册。这些手册是常见 MCP 的具体示例。同类别换一个 MCP 时，照着改写。

| 类别 | 操作手册 | 它记录的示例 MCP |
|---|---|---|
| 版本控制历史 | [`code-archaeology.md`](./sources/code-archaeology.md) | git、`gh` |
| 工单系统 | [`linear.md`](./sources/linear.md) | Linear（可改写用于 Jira、GitHub Issues、Plane、Shortcut） |
| 长文档 | [`notion.md`](./sources/notion.md) | Notion（可改写用于 Confluence、Google Docs、Coda） |
| 团队实时聊天 | [`slack.md`](./sources/slack.md) | Slack（可改写用于 Discord、Microsoft Teams、Mattermost） |
| 基础设施可观测性 | [`datadog.md`](./sources/datadog.md) | Datadog（可改写用于 New Relic、Honeycomb、Grafana、Splunk） |
| 错误与异常追踪 | [`sentry.md`](./sources/sentry.md) | Sentry（可改写用于 Rollbar、Bugsnag、Airbrake） |
| 产品分析数仓 | [`databricks.md`](./sources/databricks.md) | Databricks SQL（可改写用于 Snowflake、BigQuery、ClickHouse、dbt） |

跨类别：

- [`incident-postmortem.md`](./sources/incident-postmortem.md)。目标代码看起来是防御性的（空值检查、重试、超时、限流、功能开关、出口守卫、OOM 处理器）就加上这份。