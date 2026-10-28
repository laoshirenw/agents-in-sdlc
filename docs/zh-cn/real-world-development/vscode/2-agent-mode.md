---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次修改……类似的工作还有很多。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式能够在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。现在来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们适用于不同的体验。智能体模式在 IDE 中执行任务，可以快速反馈并随时交互。云智能体则像团队中的结对程序员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一项练习中已经添加了发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改之前，先浏览 Tailspin Toys 网站，了解它目前的功能。

此网站是一个以开发者为主题的桌游众筹平台，可用于列出游戏并显示其详情。它是一个单体 Astro app，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的精简 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可以在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。上一项练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新的终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 会自动将你转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，Web 服务器现在通过它提供服务。

### 浏览网站

网站运行后，可以探索它的功能。网站主要包含以下功能：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示更多信息，包括游戏标题、说明、发行商和类别。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的对话。每项练习所需的模式和模型可能不同，相关步骤会在适用处说明。

网站的初始实现已可正常使用，但我们想为它添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题——让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时，我们使用了 Claude Sonnet 4.5，因此将它列在说明中。这样做是希望代码建议相对一致，从而带来更好的体验。不过，大型语言模型具有概率性，因此你收到的建议可能与研讨会中的描述不同。这完全正常。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但应该仍然能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意，Copilot 还会根据它认为对用户最有用的事项为列表排序。

## 查看指令文件

启动智能体生成代码前，先查看用于向 Copilot 提供上下文的指令文件。我们将使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了向网站添加功能时应采用的方法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能时应遵循的总体指南，包括：
   - 架构概述；
   - 有关组件设计、可测试性和无障碍访问的原则；
   - 指向多种文件类型的专用指令文件的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这种方式便于重复使用内容、将复杂指令拆分成更小且易于管理的部分，以及提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用执行三项独立更改：

- 在数据层 (`src/lib/`) 中添加或完善筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，测试必须全部通过，才能将更改合并到代码库。Copilot 智能体模式可以为你完成这些任务。现在来添加功能。

1. 继续使用当前 Copilot 对话，或者选择 **New Chat** 开启新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 作为指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与功能有关的文件。你应该会看到它找到数据层辅助函数、UI 和测试。然后它会开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作过程中选择这些按钮。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 处理任务期间可能会暂停，甚至出现测试失败。这没有关系！Copilot 会交替生成代码和运行测试，直到任务完成并且不再发现错误。

   ![Copilot 智能体模式的完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，确认没有潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解到：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改；
- 如何在将代码合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建功能 PR。

### 奖励探索练习 — 实现分页

随着游戏列表不断增长，需要添加分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是使用固定值；
- 编写提示时，向 Copilot 提供所需指导，以便按预期实现功能；
- 可能需要与 GitHub Copilot 迭代，要求它修改并提供上下文。这是使用 Copilot 时的正常工作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
