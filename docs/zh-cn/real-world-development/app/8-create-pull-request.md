---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一分支上。现在一起审查这些更改并创建拉取请求。你之前手动合并了星级评分拉取请求 (PR)，这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为了简化研讨会流程，我们将完整的筛选和质量工作流保留在同一个会话和分支中，并通过一个 PR 提交所有工作。

本课将学习如何：

- 了解 Agent Merge 以及它如何自动管理合并流程；
- 查看完整的功能 PR 和验证证据；
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发者保留合并授权权。

## 了解 Agent Merge

**Agent Merge** 会自动处理在 GitHub Copilot app 中完成拉取请求所需的剩余工作。启用后，app 会话会读取拉取请求并解决阻塞项——修复失败的持续集成 (CI) 检查、回复审查评论，并在必要时执行变基——然后在 GitHub 允许时合并 PR。它会在后台运行，app 重启后仍会继续，并在拉取请求合并后自动关闭。

此前，你一直手动选择 **Merge pull request**。Agent Merge 可以代为处理，但它编辑代码和合并的权限仍需要你明确授权。授予合并权限前，请审查它可以执行的操作和已有更改。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查完毕，现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅在下拉菜单中选择该选项不会启动流程。

Agent Merge 流程开始运行。它会：

- 创建包含标题和说明的拉取请求；
- 如果会话从议题启动，则在说明正文中引用相关议题；
- 对目标分支执行变基，或处理潜在的合并冲突；
- 监控 CI 流程，确保所有检查通过；
- 监控 PR 中其他开发人员或 Copilot 代码审查的反馈，并修改内容以解决相关评论；
- 可以选择在所有操作成功后自动合并 PR。

让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，先审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并 PR：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进进度并解决报告的阻塞项。继续前确认 PR 状态为 **Merged**；仅启用合并权限或智能体正在运行，不代表 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或权限不足等限制。继续前请先解决这些阻塞项。

## 总结和后续步骤

你已自动完成开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 以及它如何自动管理合并流程；
- 查看了完整的功能 PR 和验证证据；
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，使用现有画布并创建分类画布，探索与智能体检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
