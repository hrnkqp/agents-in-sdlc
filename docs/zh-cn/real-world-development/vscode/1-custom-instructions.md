---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[说明文件][instruction-files]指导代码生成。

本练习将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目上下文、编码指南和文档标准；
- 在现有说明生效时生成筛选功能的第一块数据切片（publishers helper）；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 提交说明更新和筛选功能基础代码，并合并到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 的输出具有概率性而非确定性。本练习的重点是观察更新说明后行为的*趋势*，而不是逐字符匹配输出。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试；
- UI 应采用深色模式并具有现代感；
- 应使用 TSDoc 文档注释为代码添加文档；
- 每个文件开头都应添加注释块，说明文件的用途。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码约定、库以及希望包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级指导。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会将这个说明文件发送给 Copilot。文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如为 TypeScript 或 Astro 提供指导，或说明如何创建 UI 组件和单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，例如 [AGENTS.md 和 CLAUDE.md][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目概述、结构和全局编码标准；
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明；
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过实验，找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览本项目文件时，你会看到针对多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应界面的课程会在相关位置介绍。
>
> 需要模板或起点？请参阅 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：核心文件 `copilot-instructions.md` 以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的嵌套 **GitHub Actions Workflows** 指南。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定说明适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看为本项目创建单元测试的专门说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并向 Copilot 提供生成代码时可参考的示例。这里的路径相对于说明文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型的选择会因练习而异；每一步会在相关位置说明。

为观察自定义说明的影响，先在当前说明生效的情况下发送提示。稍后更新说明并重新发送相同提示，观察差异。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 能修改文件。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常还会修改 helper 文件及其测试。
6. 留意提议的 helper 是带类型的函数，接收 `db` 客户端作为第一个参数，并返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意提议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性；即使未明确要求，它也可能添加文档注释。即使如此也没有问题，重点仍是观察更新说明后*一致性*的变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。这里记录了项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可放在存储库级文件（`.github/copilot-instructions.md`）中提供全局指导，也可放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意新提议的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 Publisher 数据访问 helper。
    * 提供从数据库检索 publisher 信息的函数。
    */
   ```

5. 留意提议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含 id 和 name 的所有 publisher 列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 publisher 对象数组。
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一练习会继续使用这段代码。

## 提交说明并推送分支

说明文件和其他存储库资源一样，应通过版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明更新和 helper 更改均已存在：

   ```bash
   git status
   ```

3. 暂存说明更新及 Copilot 为 helper 基础所更改的所有文件，然后提交：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将它关联到待办事项中的文档标准议题。Copilot 可以通过已经连接到项目的 GitHub 工具查找议题、创建关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中的 **Agent** 已选中。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，同时说明其中还加入了后续筛选工作所需的 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果 Copilot 请求你 **Allow** 它代为执行 GitHub 操作，请审查命令并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 请求你 **Allow** 它代为执行 GitHub 操作和 shell 命令，请审查命令并酌情选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何从本项目中的说明文件获取上下文，并使用 VS Code 中的 Copilot Chat 完成以下任务：

- 发送代码生成提示，并观察现有说明对 Copilot 输出的影响；
- 向 `.github/copilot-instructions.md` 添加新的全库标准；
- 重新运行相同提示，观察提议代码如何采用新标准；
- 将说明更新和 helper 基础提交到 `main`，供下一练习继续使用。

下一步，使用[代理模式][next-lesson]，让 Copilot 在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
