---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你要求 Copilot 云智能体处理了三项不同的任务，目的是改善用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求 (PR) 的智能体任务，了解已执行的操作，甚至引导正在运行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务；
- 在会话运行期间引导智能体，要求它添加功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以趁机添加浅色模式。既然正在更新网站样式并添加切换功能，加入浅色模式也很合理。现在，你要引导智能体，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，其中可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建环境；
    - 设置 MCP 服务器；
    - 执行完成指定任务所需的各项操作。

> [!NOTE]
> 具体列出的步骤会因 Copilot 当前的工作状态和采用的方法而异。

4. 同时留意右侧显示的拉取请求 (PR) 窗格。在此处可以查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在你已了解如何查看正在执行的任务，接下来要求 Copilot 在开发高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成环境设置并开始处理任务。出现与以下内容类似的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体任务页面截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会在完成你发送消息时正在执行的步骤后，将新请求纳入工作流程。

和之前一样，这需要几分钟。此时可以暂停一下，回顾至今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心位置。在此任务控制中心，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，甚至重新引导正在运行的任务以调整范围或补充说明。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 引导正在运行的会话，要求它添加功能。

Copilot 正在完成无障碍访问功能。接下来，我们将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
