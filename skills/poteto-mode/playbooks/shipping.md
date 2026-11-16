### 发布

**落地的东西由你负责。逐个独立验证每个 PR，只落地从根部起的连续已验证段，然后别碰队列。**

这是 `playbooks/babysit.md` 之后的另一半。

1. **先确定托管平台，再逐个独立验证每个 PR。** 默认用 GitHub CLI（`gh`）。如果 `command -v origin` 成功且 Origin 能解析这个仓库，PR 的查看、盯守、编辑和合并就用 `origin pr ...`。否则留在 `gh`，并记下这次回退。绝不依赖 Graphite（`gt`）。每个 PR 一个子代理，不打包，每个都是 `delegate_task` 子代理，每个都操练真实界面，UI 用 `browser_navigate` 和浏览器工具，CLI 用 `terminal`，在 parent 与 head 上各跑一遍作对照。每个返回 `PASS`、`PASS+NOTES` 或 `FAIL`，并把结论发在自己的 PR 上。安全的定义是，结论出自没写这份代码的代理。CI 全绿不是结论，机器人审查批准也不是结论。
2. **只落地从最底部起的连续已验证段。** 从最低的未合并 PR 往上走，停在第一个没有通过结论的 PR 上，`PASS` 和 `PASS+NOTES` 都算通过。压在未验证 PR 上面的已验证 PR 不能落地。用 PR 编号报出上限，并说清链条断在哪。
3. **复查每个结论是否仍对应当前补丁。** 记下结论对应的 head SHA、base SHA，以及该 PR 从 base 到 head 的 diff 的稳定 `git patch-id`。rebase（变基）或改基分支会重写 SHA，可能不动任何检查就让结论悄悄失效。落地一个 PR 之前，把记录的 patch-id 与它当前 base 到 head 的 patch-id 比对。两个补丁只在测试、文档或 lint 配置上有差异时，把每条泳道跑过的东西构建出来。在结论 SHA 上构建两次，在当前 head 上构建一次。若结论 SHA 上的两次构建也出现同样差异，或差异是内嵌的 commit SHA，那它就是噪声。逐条差异判断，不是逐个文件判断，每类噪声连同涉及的文件一起汇报。只差噪声时，该泳道的结果仍然有效，检查和改动审查重新跑。来自开发服务器或其他没有构建产物的东西的泳道结果，不许复用。重跑那条泳道。补丁变了，其余一律重新验证。没变就保留代码结论，但在当前 head 上重跑可合并性检查和 CI。绝不拿相同的 commit 信息或旧 SHA 上的绿色检查当替代品。
4. **只准备最底部的 PR。** 拉取当前主干。需要时把最低的已验证分支 rebase 到主干的最新提交上，推送，然后只把那个 PR 的基分支改为主干，用 `origin pr edit <pr> --base <trunk>` 或 `gh pr edit <pr> --base <trunk>`。推送后重跑第 3 步。还不要改后代 PR 的基分支、给它们开自动合并或合并它们。
5. **一次落地一个 PR。** 底部 PR 现在就可合并的话，用 `origin pr merge <pr> --squash` 或 `gh pr merge <pr> --squash` 做 squash 合并。必需检查还在跑、用户又要求就绪即合并时，只给那个 PR 开启自动合并，用 `origin pr merge <pr> --squash --auto` 或 `gh pr merge <pr> --squash --auto`。Origin 的 `--auto` 是 Origin 的 merge-when-ready（就绪即合并）。GitHub 的 `--auto` 是 GitHub 的 auto-merge（自动合并）。等那个 PR 合并了再准备下一个。
6. **别把 GitHub 的 `autoMergeRequest` 读成整个堆叠已就绪。** 它至多说明某一个 GitHub PR 请求了 GitHub 自动合并。它证明不了 Origin 的就绪即合并已开启、后代 PR 已进队列、补丁结论仍然有效，或连续堆叠是安全的。向当前托管平台确认底部 PR 的状态，平台报不出来就说状态未知。
7. **每次合并后重新计算。** 拉取主干，确认合并的 SHA 已在，把已合并的 PR 从冻结的自底向上清单里去掉，检查新底部 PR 的 base、head、检查和 patch-id。托管平台可能会自动改子 PR 的基分支，但别假定它改了。对那一个 PR 重复第 3 到 6 步。独立的工作在这条链之外，自行发布。
8. **盯住当前前沿，直到它合并或失败。不要动它周围的队列。** 用 Origin 时，跑 `origin pr view <pr> --checks --comments` 和 `origin pr checks <pr> --watch`，然后反复读该 PR，直到它报出已合并或被阻塞。用 GitHub 时，`scripts/watch-pr/watch-pr --queued-stack --stack-prs <bottom>` 只当事件唤醒用，每次唤醒后轮询 `gh pr view <pr> --json state,mergedAt,mergeStateStatus,statusCheckRollup,autoMergeRequest`，忽略 `READY`，直到 `mergedAt` 非空或 `state` 为 `MERGED`。到那时才跑第 7 步。只有这些情况才判硬失败，`state` 为 `CLOSED` 且没有 `mergedAt`；必需检查以 `FAILURE` 或 `CANCELLED` 告终，且自动合并不再等待时阻塞了合并；或 `mergeStateStatus` 为 `UNSTABLE` 或 `DIRTY` 且没有待处理的自动合并。检查还在跑或自动合并已开启时的 `BLOCKED` 不算失败。这里不要用 盯跑 操作手册的排队 `WAITING`/`merge-queue` 停止条件。用动态模式的 `cronjob` 维持盯守。每次合并都汇报，连同新的上限。队列卡住时，先诊断再动手改。
9. **停在上限处。** 连续已验证段合并完后，汇报落地了什么、下一个未验证的 PR 是哪个、验证它需要什么。要延长这一段，就从第 1 步重新走一遍。

**回复：** 连续已验证段及其上限，每个 PR 的结论和出具者，你给哪些 PR 开了自动合并、怎么确认的，落地了什么，以及下一个缺口需要什么。