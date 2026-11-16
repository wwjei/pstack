### 堆叠自动驾驶

跑 全自动驾驶 操作手册（`playbooks/autopilot-full.md`），但不合并。

1. **第 5 步改为停在已验证。** 每个 PR 在当前 head 上验证通过且全绿就停，然后把 PR 交给操作者。交接之后，没有操作者发话谁也不许推送。每次审计巡检的提示词都点名这个文件，让每一轮都看到不合并规则。
2. **只有当一个 PR 建立在另一个 PR 之上时才堆叠。** 这是第 3 步唯一的例外。按 `playbooks/opening-a-pr.md` 的 **Size and stacks** 一节，把 PR 的基分支设为另一个 PR 的分支，而不是主干。第 2 步的 rebase 到主干改为，每个 PR rebase 到其父 PR 的分支，堆叠的根 PR rebase 到主干。下层 PR 出现新 head 时，上面的 PR 重新堆叠，重新堆叠后 `git patch-id` 不变的 PR 保留原结论。

**回复：** 逐个列出 PR 及其结论，代替合并结果。