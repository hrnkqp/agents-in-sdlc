---
title: "练习 4 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够访问相关指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。完成筛选功能后，现在可以了解 Copilot 使用了哪些说明，添加文档标准，并将其应用于代码。

本练习将：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体；
- 更新说明文件，确保遵循编码标准；
- 观察说明文件对代码的影响。

## 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值，包括适用时的可注入 `db` 参数；
- 可复用 Astro 组件应记录 `Props` 接口，相关代码更改时也应更新注释；
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码约定、库以及希望包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型提供任务级指导。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会将这个说明文件发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖某种格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 浏览本项目中的自定义说明文件

启动项目时已附带一组说明文件。先浏览现有文件，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此存储库中的 Copilot 每次交互都会应用这些说明。
4. 打开 `.github/instructions` 文件夹并浏览。其中包含适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了决定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和项目现有文件的链接。这样可以拆分较大的说明集、复用内容，并提供示例供 Copilot 参考。

## 更新说明以符合团队指导

现有文件是不错的起点，但仍有缺漏。修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约在第 35 行）。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选会话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选会话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确解释了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取说明文件中的上下文，并将新标准应用于功能。本练习中，你：

- 了解了存储库说明和按路径限定范围的说明文件如何提供给智能体；
- 更新说明文件以确保遵循编码标准；
- 观察了说明文件对代码的影响。

下一步，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
