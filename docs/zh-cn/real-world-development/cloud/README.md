---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理开发任务。你可以在 GitHub 上分配工作，云智能体会在后台探索存储库、修改代码并创建拉取请求，你则可以继续处理其他事情。

本系列课程将介绍如何添加云智能体遵循的自定义说明、分配 GitHub 议题并让智能体实现任务。你还将审查并使用自定义智能体来引导其工作方式，从智能体仪表板监控和指导会话，最后审查智能体创建的拉取请求并迭代改进结果。

## 课程

| 课程 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云智能体会遵循的自定义说明 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 GitHub Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并指导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查拉取请求 (PR)、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为账户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、模型上下文协议 (MCP)、代码审查和 Copilot 命令行界面 (CLI) 功能。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，Copilot Student、Pro、Pro+、Business 和 Enterprise 计划均可使用云智能体。使用 Copilot Business 或 Enterprise 时，组织管理员必须先启用该功能。

> [!NOTE]
> 本工作坊不涉及模型上下文协议 (MCP)。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 工作坊][cli]或 [VS Code 工作坊][vscode]。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
