---
title: "练习 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本练习将：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

在创建拉取请求 (PR) 前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用角色。配置文件说明智能体的用途、指令和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义项各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何使用这些能力评估需求并报告发现。它们彼此互补，无需另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。该配置将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查其配置，确认 Copilot 捕获了预期的 QA 工作流和限制。这可以避免智能体在你只想验证功能时擅自修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选的，但添加后可清晰显示智能体名称。
3. 阅读配置说明，确认 QA 会从需求开始、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置缺少任何职责或边界，请在继续前要求默认智能体修订。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置后，恢复同一个筛选对话并选择 QA，这样它就能使用已有上下文中的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了智能体，它可能不会显示在可用智能体列表中，但仍然可以通过上述命令启用。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已向工作流添加可复用的专用角色，并审查了它的工作。本练习中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作；
- 创建并检查了可复用的 QA 配置；
- 选择 QA 智能体，根据筛选议题审查其发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备就绪。下一步，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
