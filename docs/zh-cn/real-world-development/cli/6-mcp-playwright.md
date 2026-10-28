---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "查看或配置 Copilot CLI 中的 Playwright MCP，并使用它在浏览器中试用筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还需要处理数据和外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 的内置功能，访问更多工具和服务。

本课将学习如何：

- 了解 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它；
- 如果尚未安装 Playwright MCP 服务器，则添加它；
- 让智能体驱动浏览器并试用筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，仍需要实际与 UI 交互。你希望 Copilot 像用户一样使用正在开发的网站，进一步自动化更改验证流程，从而更有把握地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以与外部工具和服务实时通信，从而访问最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组 AI 智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助大家发现并为生态系统做出贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未安装 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的测试。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确告诉 Copilot 使用 MCP 服务器，它一般会自行判断。但既然你知道应该使用哪个工具，明确指出也有好处！这有助于获得更一致的结果，并节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的各项操作，以确认功能是否正常。
3. 阅读报告，确认所有行为都符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器，然后向你提供报告。

## 总结和后续步骤

恭喜，你已通过 Copilot CLI 使用 Playwright MCP 服务器在真实浏览器中试用功能。回顾一下，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它；
- 如果尚未安装 Playwright MCP 服务器，则添加了它；
- 让智能体驱动浏览器并试用筛选功能。

接下来，你将[创建 QA 自定义智能体][next-lesson]，把技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
