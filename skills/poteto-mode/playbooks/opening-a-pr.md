### 开 PR

其他每本操作手册结束时都会调用它。

**工作树。** 在从 main 拉出的 git 工作树里干活。子代理继承它。同一分支上的多次 `Task` 调用各拿一个工作树，或者在两次调用之间跑 `git fetch && git reset --hard origin/<branch>`。分支脏了、带着无关改动：导出补丁，开新工作树，再打上。工作树乱成一团：从 main 重置，最小化重做。

**提交。** 放开提交。开 PR 之前 rebase 成小而有序的提交。每个提交都是一个未来的 PR：能落地，排序能讲出故事。修复属于刚做的提交就 amend。能分开就新开提交。

**PR。** 提交前对 diff 跑 `pstack:deslop`。审查前跑 `/no-comments`。每个 PR 标题、PR 描述、提交正文都用 `/technical-writing` 写，再套 `/unslop`。technical-writing 的各层全用，只有 Diátaxis 除外。每个动作用一个词，保留冠词，普通动词够用时避免 `-ing`。

**标题。** 用 Conventional Commits，格式 `type(scope): subject`。type 用 `feat`、`fix`、`docs`、`refactor`、`test`、`chore` 或 `perf`。scope 用改动的区域，比如 `pstack` 或 `poteto-mode`。主题行短小，用祈使语气。有承载改动的真实符号就点名它。例如 `fix(pstack): retarget opening-a-pr babysit trigger`。末尾不加句点。

**描述。** PR 正文是简报，不是实验记录本。拿着 diff 的审查者应该在一分钟内弄清改动为什么存在、漏了什么、可能弄坏什么、你怎么证明它能用。写短而简单的句子，少用标识符。不要写文字墙。squash 提交的正文就是 PR 正文。正文会让 squash 提交超过约 40 行，就砍正文。

每节放在 `##` 标题下，不用加粗引语，让各节分开。按顺序用下面这些节。没话说的节就删掉。

- `## Why` 用一到三个短句给出问题和做法。不要列 SHA 或 rebase 谱系。不要加「based on main」开场白。
- `## What changed` 一到三条短要点。只有承载改动时才点名真实符号或路径。改名或改目标分支要点出两边。
- `## Scope` 永远点明 PR 覆盖什么、刻意不做什么，比如相关的后续 PR 或已知缺口。一到三条短条目。不列符号或路径，不写逐文件长文。
- `## Tradeoffs` 只点那些不写审查者就会问的被否方案。没有真正的取舍就跳过这节。
- `## Blast Radius` 用一两句话说清改动碰到谁或什么，为什么安全或有风险。main 红着的话，说清让它继续红着的代价。
- `## Verification` 一到三条要点。每条点名一条真实运行路径及其结果。性能改动报一个带单位的主数字，用 `before → after` 形式。其余证据链接 arena 或 swarm 目录。不放样本量方法论、swarm 流水账或指标表。

这些节之后，视频或截图能证明论断就附上。不要贴完整 SHA、swarm 或 arena 泳道流水账、杠杆修正长文、逐文件清单或「CLEAN」结论。这些细节放进链接的产物里。提交正文不复述主题行。

**托管平台。** 第一次 PR 操作之前定下托管平台，创建、编辑、查看、监视、合并都沿用这个选择。默认 GitHub CLI（`gh`）。`command -v origin` 成功且 Origin 能解析这个仓库时，优先 `origin pr ...`。Origin 不在或解析不了仓库，就留在 `gh`，记下这次回退。不要求 Graphite（`gt`）。

**内置 PR 工具。** 运行环境提供内置 PR 工具时，创建、编辑、改目标分支、标就绪都走它，绝不走托管平台 CLI。怎么用，它自己的说明里有。用 CLI 建的 PR 会漏掉工具跟踪的东西，比如后续运行可以编辑的描述。工具覆盖不到的都用定下的托管平台，运行环境没有这种工具时所有 PR 操作都走托管平台。

**大小与堆叠。** 宁要五个窄 PR，不要一个大 PR。堆叠是一条基分支链。根 PR 指向主干。每个子分支 rebase 到父分支的确切顶端，它的 PR 指向父分支。没有内置 PR 工具时，按定下的托管平台用 `origin pr create --status open --base <parent-branch>` 或 `gh pr create --base <parent-branch>` 建子 PR，用 `origin pr edit <pr> --base <parent-branch>` 或 `gh pr edit <pr> --base <parent-branch>` 给已有子 PR 改目标分支。只有独立工作才直接从主干开分支。大量堆叠工作之前先 rebase 到主干。

**就绪。** 每个 PR 都开成就绪，绝不开成草稿。内置 PR 工具可能默认草稿，所以每次通过它创建都设 `draft: false`。Origin 传 `--status open`。`gh` 不带 `--draft`。PR 还是开成了草稿，就通过 PR 工具标就绪，或按定下的托管平台跑 `origin pr ready <number>` 或 `gh pr ready <number>`。提到 PR 状态之前先跑 `origin pr view <number>` 或 `gh pr view <number>`。

**盯跑。** 开 PR 不启动盯跑。贴出 URL，继续构建。先做完阶段或堆叠。只有整个堆叠存在之后用户开口要，才单独跑一轮盯跑。每个新 PR 都盯跑会拖住构建，把检查花在后续波次会重启的提交上。反馈偏离意图就顶回去。

开 PR 的子代理跑 `pstack:deslop` 和 `/no-comments`，贴出 URL。然后不盯跑，直接返回父级，除非它是 全自动驾驶 或 堆叠自动驾驶 的所有者。那个所有者的任务说明里指派了盯跑循环，正是 `playbooks/babysit.md` 等的那个请求。所有者在代码就绪报告之后启动循环，并报告合并就绪。这里和 `playbooks/babysit.md` 里那些要求整个堆叠建好才盯跑的规则，对那个所有者不适用。