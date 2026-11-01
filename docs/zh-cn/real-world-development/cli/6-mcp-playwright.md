---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面提到过，编写代码不只是写代码。我们需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。这就是 MCP 服务器的用途。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

本练习将：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加它；
- 请智能体操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与界面交互。你希望 Copilot 能像用户一样使用网站，从而进一步自动化更改验证，并增强对功能符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post] 为 AI 智能体与外部工具和服务通信提供了方法。使用 MCP 后，智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，每个服务器都提供一组可供智能体访问的工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 使用 MCP 服务器时，应像使用项目中的其他依赖项一样谨慎。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，再按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 通过 Playwright 体验功能

之前你已手动确认功能是否正常。现在使用 Copilot 中的 Playwright MCP 服务器重复检查。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确告诉 Copilot 使用 MCP 服务器，它会自行选择。但既然你知道要使用哪个工具，明确指出也有帮助。这能让结果更稳定，并节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的步骤，以确认功能正常。
3. 阅读报告，确认一切符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结和后续步骤

祝贺你使用 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中体验了功能！回顾一下，你：

- 了解了模型上下文协议 (MCP) 及 Copilot CLI 如何使用它；
- 如果之前未配置，则添加了 Playwright MCP 服务器；
- 请智能体操作浏览器并体验筛选功能。

下一步，[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
