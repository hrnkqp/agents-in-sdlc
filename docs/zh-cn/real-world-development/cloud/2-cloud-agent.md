---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义说明][previous-lesson]

几乎所有组织都会遇到技术债务，例如尚未解决的问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，让开发者专注于更重要的工作，加快开发进程。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解清晰界定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有为代码编写文档。团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页显示同一类别中的其他游戏，让访客继续浏览。此项功能目前只需支持用户体验验收测试，不要求完整打磨，而且优先级暂时较低。

这两项任务都可能被不断推迟，适合交给 Copilot 云智能体处理。它可以异步工作，让开发者继续处理其他任务，稍后再审查结果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可以像人类开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始执行任务。你可以在会话进行中或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法——不能因此省略思考过程。即使看似简单的操作，也可能包含复杂细节，因此请注意[如何为 Copilot 云智能体界定任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其用途。如果你已完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没有，可以现在快速查看。在将工作分配给云智能体之前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

在 GitHub Web 界面或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将说明限定为 `**/*.test.ts` 文件。

分配 `Code lacks documentation` 议题后，查看生成的 PR，观察这些说明如何影响 TSDoc 注释、TypeScript 约定和文件注释头。后续练习还会再次检查这些内容。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，确保状态正常。将任务分配给 Copilot 时也是如此，它会像软件工程师一样完成任务。

云智能体使用 [GitHub Actions][github-actions] 执行工作。你可以通过在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，自定义它开始工作前使用的环境，使其获得所需开发工具和依赖项。实验已预先配置此工作流，以确保流程顺利并提供学习机会。它会安装 Node.js、项目依赖项、端到端测试所需的浏览器，并为单个 Astro 应用迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 Actions 选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read。如果不克隆，设置步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来像其他 GitHub 工作流文件，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流，以便测试，而无需等待 Copilot 启动。

## 添加文档

文档经常过时或缺失，这类技术债务会拖慢开发，并增加维护代码库或帮助新成员加入团队的难度。Copilot 擅长生成文档，因此很适合将这项工作分配给云智能体。它会在后台生成文档，稍后的练习中再回来审查。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它需要先创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表和 Copilot 将要完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码和理解项目状态、暂停思考并制定方案，以及编写代码。

这可能需要几分钟。云智能体的主要优势之一是能够异步执行任务，让我们腾出时间处理其他工作。现在就利用这一点：再分配一项任务给云智能体，然后转而为应用添加功能。

## 在游戏详情页添加相关游戏区域

云智能体可以拆分工作：你专注于一组任务，智能体同时处理另一组。即使为设计团队添加相关游戏区域不会花太久，也可以把这段时间用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

不久后，议题中的第一条评论旁边会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求！云智能体的工作方式与软件工程师类似，你不必一直监控它，完成后再审查即可。现在可以转而创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、开发新功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解清晰界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转而创建并使用自定义智能体。[Copilot 云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
