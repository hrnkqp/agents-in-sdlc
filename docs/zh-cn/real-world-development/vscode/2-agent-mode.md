---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行其他操作，例如运行测试。开发者往往需要先找出所有相关文件、进行修改、运行测试、调试、找出遗漏的文件，再继续修改……工作可能会不断重复。

这正是 Copilot 代理模式的用途。

Copilot 代理模式可在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 代理模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库之前审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加 publishers helper，现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站之前，先了解当前功能。

该网站是一个带开发者主题的桌游众筹平台，可列出游戏并显示详细信息。网站是单个 Astro 应用，页面在构建时渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应该已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否为 `http://localhost:4321` 打开浏览器，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 终端中选择 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前运行 Web 服务器的 Codespace 的私有隧道。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型的选择会因练习而异；每一步会在相关位置说明。

网站初始版本可以正常运行，但我们希望添加新功能。先查看待办事项。使用项目模板时已自动创建一组议题，请 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。编写工作坊时使用了 Claude Sonnet 4.5，因此将它列入说明，希望让代码建议相对一致，以获得良好体验。不过，大型语言模型的结果具有概率性，你可能会收到与示例不同的建议。这完全正常。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还根据它判断对用户最有用的程度对事项进行了排序。

## 查看说明文件

让智能体生成代码前，先查看将用于提供上下文的说明文件。项目中的[用户界面 (UI) 说明][ui-instructions]介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向各类文件专用说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用内容、将复杂说明拆分成更易管理的部分，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更改：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 代理模式可以完成这些工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉菜单中仍选择 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件，包括数据层 helper、UI 和测试。然后它会开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，这可能需要几分钟。过程中，代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。工作完成后，每项更改都会显示相应按钮，因此不必在工作进行时立即选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

祝贺你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了新功能。本练习中，你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 让代理模式探索项目、识别相关文件并协调更改；
- 在将更改合并到代码库前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为它创建拉取请求。

### 额外探索练习——实现分页

随着游戏列表不断增长，需要启用分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。考虑以下内容：

- 遵循现有最佳实践，包括使用已有说明文件；
- 决定分页方式，例如是否允许用户选择页面大小，或将其固定；
- 编写提示时提供足够指导，让 Copilot 按预期实现；
- 可能需要与 GitHub Copilot 迭代，提出更改要求并补充上下文。这是与 Copilot 协作时的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
