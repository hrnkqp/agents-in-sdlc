---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你要求 Copilot 云智能体处理了三项任务，重点是改善用户体验和添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务、检查执行情况，甚至引导正在运行的会话。

本练习将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，就适合一并完成。你将引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看所有已分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档；
   - 生成用于修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 创建 Copilot 的工作环境；
   - 设置 MCP 服务器；
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和执行方式而变化。

4. 留意右侧出现的 PR 面板。你可以在此查看 PR 及更改的文件，以便持续监控。

## 引导云智能体

现在已经查看正在处理的任务，接下来要求 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择实现高对比度模式的会话。具体标题可能因 Copilot 采用的名称和当前工作状态而异。

   ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新请求加入工作流程。

这同样需要几分钟。可以利用这段时间回顾到目前为止的学习内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有正在进行和已完成的任务、审查当前工作，甚至重新引导正在运行的任务以调整范围或提供补充指导。

本练习中，你：

- 使用 Mission control 和智能体页面监控了云智能体任务；
- 重新引导了一个正在运行的会话，请求添加额外功能。

Copilot 正在完成功能工作。接下来，[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
