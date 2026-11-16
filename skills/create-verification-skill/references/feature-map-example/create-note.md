# 创建笔记

创建笔记让用户从浏览器或 CLI 保存一条带标题的笔记，取消未完成的草稿，并从第二个用户可见视图确认已保存的笔记。

## 子功能

- `create-open` 从每个浏览器入口打开空白编辑器。
- `create-save` 持久化标题和正文。
- `create-cancel` 丢弃未完成的浏览器草稿。
- `create-cli` 从终端创建同样形状的笔记。

## 用户怎么走到这里

- 点选浏览器工具栏里的 `New note` 按钮。
- 焦点不在可编辑字段里时，在浏览器里按 `n`。
- 在终端跑 `notes create --title <title> --body <body>`。

## 用 control-notes 驱动

前置条件：

- Notes 在 `http://127.0.0.1:4173` 健康运行。
- 没有标题为 `Release checklist` 的笔记。
- `control-notes doctor` 报告预期的 URL 和一次性数据目录。

- **打开编辑器。**点选 `New note`。跑 `control-notes browser click --role button --name "New note"`。出现名为 `Note editor` 的表单，焦点落在 `Title` 文本框里。
- **输入内容。**输入标题和正文。跑 `control-notes browser fill --role textbox --name "Title" --value "Release checklist"` 和 `control-notes browser fill --role textbox --name "Body" --value "Tag and publish"`。`Save note` 按钮变为可用。
- **保存笔记。**点选 `Save note`。跑 `control-notes browser click --role button --name "Save note"`。出现名为 `Note saved` 的状态提示，标题显示为 `Release checklist`。
- **确认持久化。**回到笔记列表，重新打开这条笔记。跑 `control-notes browser click --role link --name "All notes"` 和 `control-notes browser click --role link --name "Release checklist"`。编辑器显示两个已保存的值。
- **取消草稿。**打开新笔记，输入 `Discard me`，点选 `Cancel`。跑 `control-notes browser click --role button --name "New note"`、`control-notes browser fill --role textbox --name "Title" --value "Discard me"` 和 `control-notes browser click --role button --name "Cancel"`。回到笔记列表，里面没有 `Discard me` 链接。
- **CLI 入口。**创建第二条笔记。跑 `control-notes cli -- notes create --title "CLI note" --body "Created from terminal" --format json`。退出码为 `0`，stdout 里包含新笔记的 ID 和标题。
- **证明。**从 `All notes` 重新打开两条已保存的笔记。跑 `control-notes browser snapshot --aria --path artifacts/create-note/list.aria.txt` 和 `control-notes browser screenshot --path artifacts/create-note/list.png`。产物里能看到 `Release checklist` 和 `CLI note`。

## 坑点

- 文本框持有焦点时按 `n`，输入的是字符，不会打开新编辑器。
- 标题在保存时会去掉首尾空白。断言渲染出来的标题，不是草稿输入值。
- 只有保存状态提示不足以作为证明。从列表重新打开笔记。
- 夹具清理时移除 `Release checklist` 和 `CLI note`，但保留它们的证明产物。