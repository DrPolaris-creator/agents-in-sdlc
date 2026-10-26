---
title: "课程 7 - 创建并使用质量保证 (QA) 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright 模型上下文协议 (MCP) 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，并为其定义清晰的质量保证 (QA) 流程。

在本课程中，你将：

- 了解自定义智能体如何配合说明、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

在创建拉取请求 (PR) 前，Tailspin Toys 希望一致地审查需求、代码质量、自动检查、测试覆盖范围和浏览器行为。自定义智能体可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、说明和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

之前使用的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何利用这些功能评估需求并报告发现。QA 配置文件不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查及必须遵守的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查配置文件，确认 Copilot 记录了预期的 QA 工作流和边界。这样可以避免智能体配置不完整或范围过宽，在只需要验证功能时修改实现代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 front matter。`description` 是必需项；`name` 可选，但添加后会为智能体提供清晰的显示名称。
3. 阅读配置文件说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持结论的证据、修改实现代码前会先征求同意，并且不会提交更改或创建拉取请求。
5. 如果配置文件遗漏了这些职责或边界，请让默认智能体先修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话并选择 QA，使其继续使用对话中已有的议题和规划决定。

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能尚未出现在可用智能体列表中。仍可通过上述命令启用。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你为工作流添加了一个可复用的专业角色，并审查了它的工作。在本课程中，你：

- 了解了自定义智能体如何配合说明、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其发现。

现在实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。接下来，将这些内容整合到功能 PR 中，并使用 Agent Merge。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
