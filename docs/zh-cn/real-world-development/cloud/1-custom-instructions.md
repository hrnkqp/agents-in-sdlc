---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能访问这些上下文。[说明文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目上下文、编码指南和文档标准；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 系列不同，本练习不会运行前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示说明的效果。后续练习中审查云智能体生成的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码约定、库以及希望包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会将这个说明文件发送给 Copilot。文件应包含项目级信息，也就是大多数聊天或 CLI 请求都会用到的上下文，例如技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如为 TypeScript 或 Astro 提供指导，或指导如何创建 UI 组件和单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，例如 [AGENTS.md 和 CLAUDE.md][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目概述、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
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
3. 打开 `.github/instructions` 文件夹并浏览。其中包含适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用于确定说明适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看为本项目创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并向 Copilot 提供生成代码时可参考的示例。这里的路径相对于说明文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来要添加项目级规则，要求在代码中加入 TSDoc 文档注释和文件级注释头。稍后分配议题时，云智能体会读取并应用这项规则。

先创建分支：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改善代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。这里记录了项目编码标准，但没有代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可放在存储库级文件（`.github/copilot-instructions.md`）中提供全局指导，也可放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明文件。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将这次说明更新合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。之后的练习中，云智能体就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成本工作坊，可以跳过分支步骤并直接提交到 `main`。此处保留分支流程，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目的说明文件，并向 `.github/copilot-instructions.md` 添加了一项全库标准。后续练习中，云智能体生成的拉取请求会体现这项标准。

下一步，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
