# 搜索笔记

搜索让用户按标题或正文找笔记，查看命中的笔记，并分清「没有命中」和「搜索不可用」。

## 子功能

- `search-open` 从每个受支持的浏览器入口打开搜索。
- `search-match` 返回标题和正文的命中，不改动笔记数据。
- `search-open-result` 在笔记编辑器里打开一条结果。
- `search-empty` 为没有命中的查询显示完整的空状态。
- `search-clear` 清除查询，恢复最近笔记视图。
- `search-cli` 从终端返回同样的命中笔记。

## 用户怎么走到这里

- 点选浏览器工具栏里的 `Search` 按钮。
- 焦点不在可编辑字段里时，在浏览器里按 `/`。
- 在终端跑 `notes search <query>`。

## 用 control-notes 驱动

前置条件：

- Notes 在 `http://127.0.0.1:4173` 健康运行。
- 一次性数据目录里有 `Quarterly plan`，正文是 `Draft budget`。
- `control-notes doctor` 报告预期的 URL 和数据目录。

- **工具栏入口。**点选 `Search` 按钮。跑 `control-notes browser click --role button --name "Search"`。出现名为 `Search notes` 的对话框，焦点落在它的搜索框里。
- **键盘入口。**关掉对话框，让页面获得焦点，按 `/`。跑 `control-notes browser press --key "/"`。出现同一个对话框，页面没有插入斜杠。
- **标题命中。**输入 `quarterly`。跑 `control-notes browser fill --role searchbox --name "Search notes" --value "quarterly"`。`Search results` 列表里有 `Quarterly plan`，没有 `Grocery list`。
- **正文命中。**把查询换成 `budget`。跑 `control-notes browser fill --role searchbox --name "Search notes" --value "budget"`。结果 `Quarterly plan` 仍然可见，带正文命中的摘录。
- **打开结果。**点选 `Quarterly plan`。跑 `control-notes browser click --role link --name "Quarterly plan"`。对话框关闭，编辑器标题显示 `Quarterly plan`。
- **空状态。**重新打开搜索，输入 `volcano`。跑 `control-notes browser fill --role searchbox --name "Search notes" --value "volcano"`。搜索完成后出现名为 `No matching notes` 的状态提示。
- **清除查询。**点选 `Clear search`。跑 `control-notes browser click --role button --name "Clear search"`。搜索框为空，`Recent notes` 区域取代结果列表。
- **CLI 命中。**从终端搜索。跑 `control-notes cli -- notes search "quarterly" --format json`。退出码为 `0`，stdout 里包含一个标题为 `Quarterly plan` 的对象。
- **CLI 未命中。**搜索一个不存在的值。跑 `control-notes cli -- notes search "volcano" --format json`。退出码为 `0`，stdout 是 `[]`。
- **证明。**采集有结果的状态。跑 `control-notes browser snapshot --aria --path artifacts/search/results.aria.txt` 和 `control-notes browser screenshot --path artifacts/search/results.png`。两份产物都能认出 Notes、查询词和 `Quarterly plan`。

## 坑点

- 编辑器或搜索框持有焦点时按 `/`，插入的是文本，不会打开搜索。
- 结果经过短暂防抖后才更新。等结果列表或空状态出现，不要固定睡一段时间。
- 归档笔记被排除在外，除非用户启用 `Include archived`。
- CLI 默认输出人类可读格式。要稳定断言就用 `--format json`。
- 打开结果会改变浏览器状态。证明另一个查询前先重新打开搜索。