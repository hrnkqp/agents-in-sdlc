---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改善用户体验并添加功能：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在审查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

前面多次强调，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。现在先审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改，再为团队其他成员启用审查。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，系统设置了一些安全限制：

- Copilot 对存储库仅有读取权限，并且只对用于提交代码的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受限][agent-firewall]，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体生成的第一个 PR，它用于为代码添加文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似于 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，可以借此休息一下或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加了 TSDoc 文档注释和其他文档的代码。具体更改会有所不同。

   浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像在 PR 或代码内联评论中提及团队成员一样，使用评论提及 Copilot。Copilot 会读取评论，并启动新会话处理修改。由于结果具有非确定性，无法提供唯一的标准提示；你可以要求 Copilot：

- 为每个代码文件开头添加简短说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，请求修改生成的文档。可以使用上面的任一建议，也可以提出其他希望加入代码库的文档内容。
2. 选择 **View Session**，观察 Copilot 执行修改。留意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为可审查状态，再合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为游戏详情页显示相关游戏而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 检查队列。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：** 可在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！和之前一样，可以迭代改进。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。与之前一样，在 **Conversation** 选项卡中添加评论即可请求更改；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 Mission control 请求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，特别关注以下内容：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式对应的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 检查队列。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：** 可在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认刷新页面后偏好仍然有效。

请注意，无障碍自定义智能体如何帮助 Copilot 根据无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可像之前一样在评论中提及 **@copilot**，请求更新。

## 可选练习——继续委派任务

云智能体最适合处理真实待办事项，让你能腾出时间做其他事情。可以为存储库创建更多议题并分配给 Copilot，逐步养成这种工作习惯。例如：

- 在游戏详情页创建支持者兴趣表单；
- 在游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体系列课程。通过这些练习，你：

- **检查了存储库附带的自定义说明文件**，以便之后观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用了自定义智能体**，实现高对比度和浅色模式切换以改善无障碍体验；
- **使用 Copilot 智能体页面作为控制中心**，监控并引导无障碍会话；
- **审查并迭代了云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体系列。如果想继续探索，其他系列课程可以补充本系列所学内容：

- 🖥️ **[VS Code 系列](../../vscode/)** — 直接在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 系列](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
