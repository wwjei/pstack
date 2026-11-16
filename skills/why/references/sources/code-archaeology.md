# 代码考古（git 与仓库内）

## 这个来源里有什么

- 提交历史（提交信息、日期、作者、diff）
- PR 描述、审查评论和讨论串（通过 `gh`）
- 代码内注释、TODO、FIXME、弃用说明
- ADR（架构决策记录），如果仓库有保留
- 测试。测试名和断言常常编码了促成一次改动的边界情况
- 同一批提交里一起改的相关文件（共同变更信号）
- CHANGELOG 条目、仓库内的发布说明
- 提交信息和 PR 正文里提到的议题/工单 ID

最可信的来源，直接绑在代码上，也最完整。凡是经过仓库的东西都应该在这里。

## 怎么搜

扩展种子提交清单：

```bash
# Full history of the file through renames
git log --follow --oneline -- <file>

# Pickaxe: commits that added or removed this exact text
git log -S '<exact_string_from_code>' -- <file>

# Or for patterns:
git log -G '<regex>' -- <file>

# Who wrote each line and when
git blame -L <start>,<end> <file>

# The full diff of a specific commit
git show <hash>

# Commits between two points affecting this file
git log <old>..<new> -p -- <file>
```

对每个实质性提交，拉出 PR 背景：

```bash
# Find the PR number from the merge commit or branch
git log -1 --format=%B <hash>

# Full PR context: body, review comments, linked issues
gh pr view <number> --json title,body,author,createdAt,mergedAt,labels,closingIssuesReferences,comments,reviews,files

# The --json reviews and comments fields are where the real signal is
```

找一找散落在代码之外的文档：

```bash
# ADRs often live in docs/adr/ or similar
rg -l -i 'architecture.decision' --glob '*.md'

# TODOs and FIXMEs near the target
rg -n -C2 '(TODO|FIXME|HACK|XXX|NOTE)' <target_file>

# Related tests. Names often encode the "why"
rg -l '<symbol>' --glob '*test*'
```

## 这里的好证据长什么样

- 一份解释所解问题的 PR 描述，而不只是描述改动（「这修掉了导致 X 的分页缺陷」）
- 一条辩论过备选方案的长审查讨论串
- 目标行附近解释某个不显眼约束的代码内注释
- 一个名为 `test_handles_edge_case_when_X` 的测试，揭示促成这段代码的边界情况
- 一条引用工单或事故 ID 的提交信息
- 一条总结用户可见理由的 CHANGELOG 条目

## 常见坑点

- **压缩合并后的平地。** 仓库若对 PR 做压缩合并（squash merge），分支历史里的单个提交就没了。退回看 PR 正文和评论。
- **误导性的提交信息。** 「Small refactor」有时藏着一次有意的行为变更。看 diff，别看信息。
- **盲目照抄的模式。** 作者可能没弄懂为什么就抄了一个模式。查这个模式是否更早出现在代码库里，去调查*那个*提交。
- **机器人提交与自动合并。** Dependabot、Renovate 和自动向后移植（backport）通常不带动机。找意图时跳过它们。
- **把代码当成意图的证据。** 代码本身不是它为何存在的证据。证据来自提交信息、PR、注释、测试、文档。别拿「这个函数叫 X」当意图的证据。

## 返回什么

每个与问题相关的提交/PR/评论，附上：
- 原文（照引）
- 哈希 / PR 编号 / 文件:行号
- 作者与日期
- 是直接证据（明确回应了问题）还是间接证据