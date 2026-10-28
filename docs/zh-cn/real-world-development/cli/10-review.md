---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可重复使用的自定义项和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定实现方案，在 Autopilot 模式中构建功能，并在 Interactive 模式中审查；
- 使用自定义指令引导智能体、自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中试用筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流：**实现筛选、更新指令并将它们应用到功能中、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为了简化研讨会流程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动测试、自己在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前综合了这些结果、需求审查和测试覆盖率。

新增测试应解决实际覆盖缺口；如果 QA 检查后认为不需要新增测试，也可能是正确的。授权合并前，应审查代码和证据；更改后，应刷新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个对话中重复使用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考实现方案，使用 **Interactive** 协作完成范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。常规编辑可以选择响应更快的模型，复杂任务则可以选择能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建的内容、原因和方式，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以进一步了解以下 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获得不同视角；
- 使用 `/fleet` 并行协调独立子任务；
- 使用 `/worktree` 隔离单独任务；
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

熟练使用工具的最佳方式就是持续实践！可以用它编写生产代码、兴趣项目，或者多年来一直想构建却没有开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想继续在终端中扩展 Tailspin Toys，可以接着学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
